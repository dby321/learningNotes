#  Vuex

##  Vuex概述

> Vuex是实现组件全局状态（数据）管理的一种机制

![1599558693669](C:\Users\dby\AppData\Roaming\Typora\typora-user-images\1599558693669.png)

- 集中管理，易于开发和后期维护
- Vuex的数据是响应式的，实时保持数据与页面的同步

## Vuex的基本使用

1. 安装依赖包
2. 导入vuex包
3. 创建store对象

## State

`this.$store.state.共享数据名称`

共享数据不能直接修改



```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

```js
computed:{
    ...mapState(['count'])
}
```





## Mutation

`this.$store.commit("add")`

```js
mutations: {
    add(state) {
      // 变更状态
      state.count++
    }
  }
```

`this.$store.commit("addN",3)`

```js
mutations: {
  addN (state, n) {
    state.count += n
  }
}
```



```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

> 不要在mutation中写异步操作

## Action

> 用于处理异步任务

> Action 函数接受一个**与 store 实例具有相同方法和属性的 context 对象**

```js
actions: {
  addAsync (context) {
    setTimeout(() => {
      context.commit('add')
    }, 1000)
  }
}
```

`this.$store.dispatch('addAsync')`

> action携带参数和mutation一样



```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```



## Getter

> 用于对Store中的数据进行加工处理形成新的数据，但Store原数据不变

```js
getters:{
    showNum(state){
        return "当前最新的数量是"+state.count
    }
}
```

`this.$store.getters.showNum`





```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```