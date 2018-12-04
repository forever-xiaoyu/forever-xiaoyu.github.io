---
title: Two Sum
date: 2018-08-15 20:44:02
tags: 
categories: Others
---

## eg
Input: nums = [2, 7, 11, 15], target = 9
Output: [0, 1]

## idea
The direct way to see this is to traverse the summation through two layers of loops.
This is one of the solutions, but it consumes performance.
<!-- more -->

We can think in reverse, if the sum of the two numbers meets the conditions, then the two numbers must exist, or the value must exist as their attribute. for example, we can use value in the nums as ths key to stored in the temporary object, 2 and 7 satisfy the condition, so obj[2] and obj[7] must exist, so obj[9-7] must exist, and it is naturally solved.

## solution
1.
```
function getResult(nums, target) {
    for(var i  = 0; i < nums.length - 1; i++) {
        for(var j = i + 1; j < nums.length; j++) {
            if(nums[i] + nums[j] == target) {
                return [i,j];
            }
        }
    }
    return [];
};
```

2.
```
function getResult(nums, target) {
    var ans = [];
    var exist = {};
    for (var i = 0; i < nums.length; i++){
        if (typeof(exist[target-nums[i]]) !== 'undefined'){
            ans.push(exist[target-nums[i]]);
            ans.push(i);
        }
        exist[nums[i]] = i;
    }
    return ans;
};
```

![code](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRWaFTrsA1HV7tu3AmiHaPdDVf_HYOf_lxtDYrjwRR6ctJu-oYx "code")