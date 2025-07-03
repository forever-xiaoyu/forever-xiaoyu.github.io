---
title: iOS App 内 Apple 支付功能（IAP）的实现方案
date: 2025-07-03 22:00:03 +0800
tags: uniapp
categories: 技术
---

# 概述
苹果应用内购买（In-App Purchase，简称IAP）是iOS应用获取收入的主要方式。通过IAP，用户可以在应用内购买虚拟商品、会员服务等数字内容。本文将详细介绍IAP的业务流程和核心实现代码。

# 苹果IAP产品类别介绍
苹果应用内购买（In-App Purchase，IAP）提供了四种不同类型的产品，每种产品适用于不同的业务场景：

1. 消耗型商品（Consumable）
   
    特点：使用一次后会被消耗，需要再次购买<br>
    示例：游戏内货币（金币、宝石）、游戏道具、额外生命值、临时加成<br>
    购买方式：每次需要都要重新购买<br>
    数据恢复：不支持恢复购买，不能跨设备同步<br>
    使用场景：适合需要反复购买的虚拟物品

2. 非消耗型商品（Non-Consumable）
   
    特点：一次购买永久拥有，不会过期或减少<br>
    示例：解锁完整游戏功能、去除广告、高级版本升级、额外游戏关卡<br>
    购买方式：只需购买一次<br>
    数据恢复：支持恢复购买，可在用户所有设备上使用<br>
    使用场景：适合一次性付费解锁永久内容或功能

3. 自动续期订阅（Auto-Renewable Subscription）
   
    特点：允许用户在一定时间内访问动态内容，除非用户取消否则会自动续期<br>
    示例：音乐或视频流媒体服务、内容订阅、云存储服务<br>
    购买方式：定期自动扣费（每周、每月、每季度或每年）<br>
    数据恢复：支持恢复，可在家庭共享中分享<br>
    使用场景：适合提供持续更新内容或服务的应用

4. 非续期订阅（Non-Renewing Subscription）

    特点：允许用户在有限时间内访问内容，不自动续期<br>
    示例：访问一年的存档文章、限时解锁特定内容<br>
    购买方式：每次订阅到期后需手动再次购买<br>
    数据恢复：支持在原设备上恢复<br>
    使用场景：适合提供固定时长内容访问权限的应用

# 业务流程
## 整体流程
IAP支付的业务流程如下：
1. 获取产品信息：从后端获取可购买的产品列表，包含价格、时长等信息
2. 获取苹果支付通道：调用uni-app提供的接口获取苹果支付通道
3. 获取苹果产品列表：通过产品ID从App Store获取实际的产品信息和价格
4. 创建订单：在后端创建支付订单，获取订单号
5. 处理未完成的订单：处理可能存在的未完成交易
6. 发起支付：调用苹果支付接口，发起IAP支付
7. 验证票据：支付成功后，将票据发送到后端进行验证
8. 关闭订单：验证成功后，通知苹果服务器关闭订单

## 数据流向
1. App → 后端服务器：获取产品列表
2. App → App Store：获取苹果产品信息
3. App → 后端服务器：创建支付订单
4. App → App Store：发起支付请求
5. App Store → App：返回支付结果和票据
6. App → 后端服务器：验证票据
7. 后端服务器 → 苹果验证服务器：二次验证票据
8. 后端服务器 → App：返回验证结果
9. App → App Store：关闭订单
    
# 核心代码实现
## ApplePayHandler 类
这是处理苹果支付的核心类，封装了支付的各个环节：
```vue
import { validatePaymentResultApi } from '@/service/vip/apple-pay'

export class ApplePayHandler {
    constructor({ productIds = [] }) {
        this.productIds = productIds // 产品id
        this.iapChannel = null // 支付通道
        this.txPayResult = [] // 已支付未验证完成的交易
    }
    
    // 获取支付通道
    getIapChannel() {
        return new Promise((resolve, reject) => {
            uni.getProvider({
                service: 'payment',
                success: (res) => {
                    const iapChannel = res.providers.find((channel) => {
                        return channel.id === 'appleiap'
                    })
                    
                    if (iapChannel) {
                        this.iapChannel = iapChannel
                        resolve(iapChannel)
                    } else {
                        reject('当前环境不支持使用IAP支付！')
                    }
                },
                fail: (err) => {
                    reject(err)
                },
            })
        })
    }
    
    // 通过支付通道获取产品列表
    requestProduct(productIds) {
        return new Promise((resolve, reject) => {
            this.iapChannel.requestProduct(
                productIds || this.productIds,
                (res) => {
                    console.log('requestProduct', res)
                    resolve(res)
                },
                (err) => {
                    console.log('requestProduct err', err)
                    reject(err)
                }
            )
        })
    }
    
    // 请求支付，传递产品信息
    requestPayment({ productid, username }) {
        return new Promise((resolve, reject) => {
            const orderInfo = {
                productid,
                username,
                quantity: 1,
                manualFinishTransaction: true,
            }
            
            uni.requestPayment({
                provider: 'appleiap',
                orderInfo,
                success: (res) => {
                    console.log('requestPayment', res)
                    resolve(res)
                },
                fail: (err) => {
                    console.log('requestPayment err', err)
                    reject(err)
                },
            })
        })
    }
    
    // 验证票据
    async validatePaymentResult(tx) {
        const res = await validatePaymentResultApi(tx)
        
        if (res?.returnCode == 200) {
            return res
        }
        
        return Promise.reject(res)
    }
    
    // 关闭订单
    finishTransaction(transaction) {
        console.log('finishTransaction', transaction)
        return new Promise((resolve, reject) => {
            this.iapChannel.finishTransaction(
                transaction,
                (res) => {
                    console.log('finishTransaction res', res)
                    resolve(res)
                },
                (err) => {
                    // 无论是否成功都要resolve，这只是告知苹果服务器关闭订单，具体结果取决于苹果服务器
                    console.log('finishTransaction err', err)
                    resolve(err)
                }
            )
        })
    }
    
    // 获取未关闭的订单
    restoreCompletedTransactions(username) {
        return new Promise((resolve, reject) => {
            this.iapChannel.restoreCompletedTransactions(
                {
                    manualFinishTransaction: true,
                    username,
                },
                (res) => {
                    console.log('restoreCompletedTransactions', res)
                    resolve(res)
                },
                (err) => {
                    console.log('restoreCompletedTransactions err', err)
                    reject(err)
                }
            )
        })
    }
    
    // 处理未关闭的订单
    async dealCompletedTransactions(transactionList = []) {
        console.log('dealCompletedTransactions', transactionList)
        if (transactionList?.length) {
            const TransactionState = {
                purchasing: '0', // 应用商店正在处理的交易
                purchased: '1', // 成功处理的交易
                failed: '2', // 失败的交易
                restored: '3', // 已经购买过该商品
                deferred: '4', // 在队列中，但其最终状态为等待外部操作
            }
            
            for (const tx of transactionList) {
                switch (tx.transactionState) {
                    case TransactionState.purchased:
                        // 对于已购买但未关闭的订单，直接关闭
                        await this.finishTransaction(tx)
                        break;
                    case TransactionState.failed:
                        await this.finishTransaction(tx)
                        break;
                }
            }
        }
    }
}
```

# 注意事项
1. 开发环境配置：需要在HBuilder manifest中勾选Apple应用内支付，再进行调试。
2. 产品配置：必须在App Store Connect中正确配置IAP产品，包括产品ID、价格、描述等。开发过程中只要产品状态为准备提交即可，首次发布的产品需要和APP一同提交审核，通过后上架的APP才可正确获取到产品信息。
3. 票据验证：应在后端进行票据验证，防止伪造支付请求。
4. 订单处理：必须正确处理未完成的交易，避免用户支付成功但未获得商品。这里使用的产品类型为消耗型产品，即使验证票据了，但未关闭的订单不会返回username，后台无法获取到是哪个用户的哪个订单，所以如果出现已支付未验证的情况，让用户联系客服提供订单及账号，然后后台验证后再发放权益。
5. 交易状态：了解不同的交易状态（purchasing、purchased、failed、restored、deferred）及其处理方式，根据实际业务需求做不同的处理。
6. 错误处理：完善的错误处理机制，提供友好的用户提示。

# 开发中碰到的问题
## 获取苹果产品列表信息
为了保证价格的一致性，ios端要使用苹果产品的价格信息，但是在调用苹果产品价格信息时，发现查询的时间略长，第一次查询过后，大概会有20分钟左右的缓存，这期间再次拉取速度很快。针对第一次查询产品价格信息的情况，要添加一个loading占位，防止用户体验不佳。

## 沙盒账号支付
ios端无论是开发环境还是testflight环境都无法进行真实支付的验证，都要使用沙盒账号支付，支付后直接支付成功，所以对于掉单这种特殊情况没有办法做进一步的调试，沙盒账号是不能直接登录app store查看账号信息的，只能等上app上架后线上环境验证。因此，为了确保安全以及支付流程的正确性，建立了一个白名单机制，只小范围的添加了部分账号，只有在白名单内的用户才可以发起支付，否则会给出提示。

## 本地没有响应要移除的事务
在拉取未关闭订单的流程中，尝试关闭订单时，出现了“undefined.Payment_appleiap:本地没有响应要移除的事务”的错误，导致支付流程不能继续往下执行，解决方案是把此类异常当作正确逻辑处理，finishTransaction只是告知苹果服务器关闭订单，具体结果取决于苹果服务器。

# 总结
苹果IAP支付实现需要前后端配合，严格按照苹果的规范进行开发。本文介绍的实现方案基于uni-app框架，通过ApplePayHandler类封装了IAP支付的各个环节，简化开发流程。正确实现IAP不仅可以提高用户体验，也是符合苹果应用商店规则的必要条件。在实际开发中，还需要注意支付安全、用户体验以及各种异常情况的处理，确保支付流程的顺畅和安全。

# 参考
1. [苹果应用内支付 - uniapp](https://uniapp.dcloud.net.cn/api/plugins/payment.html#iap)
2. [有关uniapp的苹果应用内支付调起代码编写经验分享 - DCloud问答](https://ask.dcloud.net.cn/article/40044)

