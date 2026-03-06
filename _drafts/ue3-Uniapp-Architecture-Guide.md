# 📖 Vue3 / Uniapp 跨端工程化与核心架构指南

## 核心指导思想
本项目架构严格遵循 **Feature-based（按功能模块划分）** 与 **视图/模型彻底解耦 (View-Model Separation)** 原则。
核心目标是降低跨端开发（PC/H5/Uniapp）的心智负担，提升核心业务逻辑的复用率，并彻底规避由于样式注入或状态管理混乱导致的性能与维护灾难。

---

## 📂 一、 标准目录架构规范

项目根目录下的 `src/` 必须遵循以下业务域和功能层级的划分原则：

```text
src/
├── api/             # 接口请求中心 (Axios/Request 封装与声明)
├── assets/          # 静态媒体资源 (中心化管理：按业务域分子目录，如 img/order/)
├── components/      # 全局公共组件
│   ├── base/        # 纯 UI 无状态组件 (如二次封装的按钮、弹窗)
│   └── biz/         # 跨页面复用的业务实体组件 (如 OrderCard)
├── composables/     # 组合式函数 (Vue Hooks，仅处理 UI 状态与生命周期复用)
├── core/            # 🌟 核心业务逻辑层 (纯 TS Class，脱离 Vue 运行，跨端复用基石)
├── plugins/         # 插件装配车间 (UI 库按需引入、JSBridge 初始化)
├── router/          # 路由配置及全局路由守卫
├── stores/          # Pinia 状态管理 (🌟 必须按实体划分，严禁按页面划分)
├── styles/          # 样式系统与主题配置 (SCSS/LESS)
├── types/           # 全局 TypeScript 类型定义 (.d.ts)
├── utils/           # 纯基础工具方法 (正则、格式化、防抖节流)
└── views/           # 🌟 页面视图 (必须按业务模块域纵向划分，严禁扁平化平铺)
```

---

## 🎨 二、 样式系统与静态资源基建

### 1. 样式目录的职责划分与 Vite 注入铁律
样式分为**“产生实体 CSS 代码的”**和**“纯预处理静默代码的”**，必须严格隔离。

* **`theme.less` / `base.less`**：
    * 存放原生 CSS 变量 (`:root { --color: #fff; }`)、全局标签基础样式、现代 H5 Reset（如 `100dvh`、`-webkit-tap-highlight-color: transparent`）。
    * **挂载方式**：通过 `src/styles/index.less` 统一聚合后，在 `main.ts` 中仅引入一次。
* **`variables.less` / `biz-variables.less` / `mixins.less`**：
    * 存放 LESS/SCSS 预处理变量 (`@primary-color: var(--color);`) 和混入函数。
    * **挂载方式**：通过 Vite 的 `css.preprocessorOptions.less.additionalData` 全局自动注入到每个 `.vue` 组件。
    * **⛔ 绝对禁区**：注入文件中**绝对不允许**出现任何 `:root` 或实体 CSS 选择器，否则会导致编译后产物呈指数级膨胀。

### 2. 静态资源（Assets）中心化策略
为了避免深层相对路径地狱（`../../../`），放弃在 `views` 的深层目录下存放图片。
所有静态图片集中收口于 `src/assets/img/`，并在内部按业务域建包（如 `src/assets/img/order/`、`src/assets/img/user/`）。在组件中统一使用 Vite 别名 `@/assets/...` 引入。

---

## 🧠 三、 视图与核心逻辑彻底解耦 (View-Model Separation)

这是应对复杂 PC 业务、跨端小程序与 H5 的终极解法。严禁将复杂的业务计算、校验和数据格式化混杂在 Vue 组件中。

### 1. 核心模型定义 (Core 层)
在 `src/core/业务域/` 下建立纯 TS 类。它负责清洗 API 返回的脏数据（防腐层），并封装业务行为。
```typescript
// src/core/order/Order.model.ts
export class OrderModel {
  public id: string;
  public status: number;
  private _price: number;

  constructor(data: any) {
    this.id = data.order_id || '';
    this.status = data.state ?? 0;
    this._price = data.amount || 0;
  }

  // UI 直接消费此 Getter，无需在 Vue 模板里写映射字典
  get statusText(): string {
    const map: Record<number, string> = { 1: '待支付', 2: '已支付' };
    return map[this.status] || '未知';
  }

  // 纯业务行为判断
  public canCancel(): boolean {
    return this.status === 1;
  }
}
```

### 2. 视图层消费 (Views 层)
Vue 组件退化为“纯粹的渲染器”与“事件分发器”。
```vue
<script setup lang="ts">
import { ref } from 'vue';
import { OrderModel } from '@/core/order/Order.model';

const orderDetail = ref<OrderModel | null>(null);

// 初始化时，将接口返回的 Raw Data 实例化为防腐层模型
const init = async () => {
  const res = await fetchApi();
  orderDetail.value = new OrderModel(res.data); 
};
</script>

<template>
  <div v-if="orderDetail">
    <span>{{ orderDetail.statusText }}</span>
    <button v-if="orderDetail.canCancel()">取消订单</button>
  </div>
</template>
```

---

## 🗄️ 四、 状态管理 (Pinia) 与路由规范

### 1. Pinia 实体化法则
Pinia 充当“前端本地数据库”，**必须按数据实体/业务域划分**，绝对不能按页面划分。
* ✅ 正确：`useOrderStore.ts` (管理列表缓存、详情缓存、订单字典表)
* ❌ 错误：`useOrderListStore.ts`、`useOrderDetailStore.ts`

### 2. 路由与文件命名规范
* **页面级 (Views)**：采用 `views/业务域/具体页面/index.vue`（如 `views/order/detail/index.vue`），以保持路由配置的极度整洁与目录的强可扩展性。
* **组件级 (Components)**：必须具名，如 `OrderCard.vue`，方便全局搜索与 IDE 标签辨识。

---

## ⚡ 五、 复杂交互与物理副作用隔离

当面临流式输出（打字机）、长列表滚动等复杂交互时，必须严格隔离“业务数据流”与“DOM 物理副作用”。

**架构红线**：
1.  **纯逻辑引擎层 (`core/ai/AIChatEngine.ts`)**：负责 WebSocket 通信与文本拼接，绝不能包含 `document.getElementById` 或操作 `ref.value` 滚动条的代码。
2.  **UI 副作用层 (`views/ai-chat/index.vue`)**：利用 **`ResizeObserver`** 监听容器高度变化来触发 `scrollToBottom`。这既实现了打字机跟随，又解耦了业务流，同时顺带解决了图片异步加载导致的高度塌陷问题。
