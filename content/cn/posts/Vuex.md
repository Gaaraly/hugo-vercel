---
title: 'Vuex - The State Management Framework for Vue.js'
date: 2022-02-10 19:36:00
categories: ['theory']
---
# 介绍
## Vuex 是什么
Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式 + 库**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
## 安装
使用 npm 下载即可。

```
npm install vuex@next --save
```

## 开始
Vuex 的核心应用就是 store。Store 相当于一个容器，包含 [[Vuex#State|state]]、[[Vuex#Mutation|mutaion]]、[[Vuex#Action|action]]、[[Vuex#Getter|getter]] ：
```js
import { createStore } from 'vuex'
const store = createStore({
    state(){
        return{
            //...
        }
    }
    mutations: {
        //...
    }
    actions: {
        //...
    }
    getter: {
        //...
    }
})
```

- Vuex 的状态存储是响应式的，当 store 中的状态发送变化时（即 state 更新），相应的组件也会相应的及时更新。
- 我们不能直接 state 中的数据，改变 store 中的状态的**唯一**途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化

# 核心概念
## State
State 是 Vuex 存储状态的地方，state 是 store 全局共享的数据。

读取的数据的方法有两种：

1.  通过 `this.$store.state.param` 读取数据.
2.  通过 `mapState` 映射到组件自身，作为本地数据使用：

```js
import { mapState } from 'vuex'

export default {
    //...
    computed: {
        ...mapState(['param'])
    }
}
```

## Mutation
更改 Vuex 的 store 中的状态的唯一方法是**提交**(commit) mutation。Vuex 中的 mutation 非常类似于事件，它回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数。

注意，一切改变 state 的操作均只能由 mutation 来执行。

获取的 mutation 的方法和 state 相似也有两种：

1.  通过 `this.$store.commit('methodName',param)` 获取。
2.  通过 `mapMutaions` 映射到组件自身，作为本地数据使用：

```js
import { mapMutations } from 'vuex'

export default {
    //...
    methods: {
        ...mapMutations(['methodName'])
    }
}
```

可以在触发 mutations 的时候传递参数：

```js
    mutations: {
        addN(state, step){
            state.count += step
        }
    }
```

## Action
Action 类似于 mutation，**不同**在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意**异步**操作。

Action 接受 context 作为第一个参数，获取 action 的两种方法：

1.  通过 `this.$store.dispatch('methodName',param)` 获取。
2.  通过 `mapAcions` 映射到组件自身，作为本地数据使用：

```js
import { mapActions } from 'vuex'

export default {
    //...
    methods: {
        ...mapActions(['methodName'])
    }
}
```

可以在触发 actions 的时候传递参数：

```js
    actions: {
        addNAsync(context, step){
            setTimeout(()=>{
                context.commit('addN',step)
            },1000)
        }
    }
```

## Getter
有时候我们需要对 store 中的数据进行加工处理产生**新的数据**，Vuex 允许我们在 store 中定义“getter”。

- Getter 可以对 store 中已有的数据加工处理之后形成新的数据，类似于 Vue 的计算属性。
- Store 中数据发送变化时，Getter 的数据也会随之变化。

定义 Getter：

```js
    getters:{
        showNum: state => {
            return '当前为'+state.count
        }
    }
```

使用 getters 有两种方式：

1.  通过 `this.$store.getters.name` 获取。
2.  通过 `mapGetters` 映射到组件自身，作为本地数据使用：

```js
import { mapGetters } from 'vuex'

export default {
    //...
    computed: {
        ...mapGetters(['showNum'])
    }
}
```

# 杂谈
当 Vue 组件中需要传递数据时，可以通过箭头函数定义一个新的函数传递 ele 和需传递的值。
