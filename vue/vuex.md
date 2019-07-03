# vuex

> 一个专为 Vue.js 应用程序开发的状态管理模式。主要用于多个页面依赖、改变同一个状态

## 安装和引入
```安装
(c)npm install vuex --save
```
```引入
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```
## 核心介绍 {docsify-ignore}

### State

用来声明单个状态的状态树。也可以说是这个状态树就是对象，而单个状态为状态树的属性。

```store实例
const store = new Vuex.Store({
  state: {
    name: '小迷',
    age: 18
  }
})
```
* `store.state`获取状态对象
  * 页面上使用`this.$store.state.name`即可获取`name`的状态

`Vuex` 的状态存储主要的就是响应式，可以将 store 实例直接注入到所有的子组件：

```
new Vue({
  store
})
```

从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态：

```demo
<template>
  <div>
    <h1>{{ name }}</h1> // 小迷
  </div>
</template>
<script>
export default {
  computed: {
    name () {
      return this.$store.state.name
    }
  }
}
</script>
```

### mapState 辅助函数

当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性

```mapState的使用
<template>
  <div>
    <h1>我叫{{ name }}，今年{{ age }}岁</h1> // 我叫小迷，今年18岁
  </div>
</template>
<script>
// 在单独构建的版本中辅助函数为 Vuex.mapState，通过import引入
import { mapState } from 'vuex'
export default {
  computed: mapState({
    name: state => state.name,
    age: state => state.age
  })
}
</script>
```
可以通过 es6 `对象展开运算符`简写
```
computed: {
  ...mapState(['name', 'age'])
}
```

### Getter
 
有时候我们需要从 store 中的 state 中派生出一些状态，通过 getter 可以处理 state 中的状态返回新的状态内容。而要想改变 getter 的状态则就需要改变 state 的状态。相当于 getter 的状态依赖于 state 的状态。
```store实例
const store = new Vuex.Store({
  state: {
    name: '小迷',
    age: 18
  },
  getters: {
    getName: state => {
      return '我叫' + state.name
    }
  }
})
```
```demo
<template>
  <div>
    {{ name }} // 我叫小迷
  </div>
</template>
<script>
export default {
  computed: {
    name () {
      return this.$store.getters.getName
    }
  }
}
</script>
```
可以接受第二个参数，获取 getter 中的其他状态
```store实例
getters: {
  getName: state => {
    return '我叫' + state.name
  },
  introduce: (state, getters) => {
    return getters.getName + '，今年' + state.age + '岁'
  }
}
```
```demo
<template>
  <div>
    {{ introduce }} // 我叫小明，今年18岁
  </div>
</template>
<script>
export default {
  computed: {
    introduce () {
      return this.$store.getters.introduce
    }
  }
}
</script>
```

### mapGetters 辅助函数

通过mapGetters 辅助函数可以将 store 中的 getter 映射到局部计算属性

```mapGetters的使用
import { mapGetters } from 'vuex'

export default {
  computed: {
    // 通过数组格式 getter 属性直接命名
    ...mapGetters([
      'getName',
      'introduce'
    ])
    // 通过对象格式 getter 属性另取名字
    ...mapGetters({
      'getName1': 'getName'
      'introduce2': 'introduce'
    }
  }
}
```

### Mutation

用于更改 state 中的状态 `store.commit(事件)`，其中`(事件)`为 mutations 中定义的对应回调函数
```store实例
const store = new Vuex.Store({
  state: {
    age: 18
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.age++
    }
  }
})
```
```demo
<template>
  <div>
    <h1>{{ age }}</h1> // 18++
    <button @click="increment">点一下长一岁</button>
  </div>
</template>
<script>
export default {
  computed: {
    age () {
      return this.$store.state.age
    }
  },
  methods: {
    increment () {
      this.$store.commit('increment')
    }
  }
}
</script>
```
可以传第二个参数，一次长10岁，使用方法为`store.commit('increment', 10)`，事件如下：
```store实例
mutations: {
  increment (state, n) {
    // 变更状态
    state.age++n
  }
}
```
##### 注意：Mutation 需遵守 Vue 的`响应规则`且必须是`同步函数` {docsify-ignore}

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

  1. 最好提前在你的 store 中初始化好所有所需属性。
  2. 当需要在对象上添加新属性时，可以使用 `Vue.set(对象名, 对象属性, 对象属性的值)` 或者 `this.$set(对象名, 对象属性, 对象属性的值)`

### mapMutations 辅助函数

mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用

```mapMutations的使用
import { mapMutations } from 'vuex'

export default {
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment', n)`
      'add': 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    ])
  }
}
```

### Action

* Action 自己无法变更状态，需要通过 mutation 变更状态
* 可以包含任意异步操作
* 通过`store.dispatch`方法触发

```store实例
const store = new Vuex.Store({
  state: {
    age: 18
  },
  getters: {
    age: state => state.age
  },
  mutations: {
    increment (state, n) {
      // 变更状态
      state.age += n
    }
  },
  actions: {
    increment ({ commit }) {
      setTimeout(() => {
        commit('increment', 10)
      }, 1000)
    }
  }
})
```
```demo
<template>
  <div>
    <h1>{{ age }}</h1> // 18+=10
    <button @click="increment">点一下延迟1秒长10岁</button>
  </div>
</template>
<script>
export default {
  computed: {
    age () {
      return this.$store.getters.age
    }
  },
  methods: {
    increment () {
      this.$store.dispatch('increment')
    }
  }
}
</script>
```

### mapActions 辅助函数

mapActions 辅助函数将组件的 methods 映射为 store.dispatch 调用

```mapActions的使用
import { mapActions } from 'vuex'

export default {
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`
      'add': 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    ]),
  }
}
```

### Module

当状态较多时，应用复杂，如果所有状态都集中到一个对象上，则 store 实例对象变到臃肿。从而 store 将 state、mutation、action、getter 分割成模块，每个模块都拥有自己都一份。甚至可以出现嵌套关系(嵌套关系略微复杂暂未使用)。为了方便查看、调用我这里将 getters 提出来了。

```store实例
const user = {
  state: {
    name: '小明'
  },
  mutations: {
    GET_NAME: (state, name) => {
      state.name = name
    }
  },
  actions: {
    editName ({ commit }, name) {
      setTimeout(() => {
        commit('GET_NAME', name)
      }, 1000)
    }
  }
}
const user2 = {
  state: {
    name2: '小红'
  },
  mutations: {
    GET_NAME2: (state, name2) => {
      state.name2 = name2
    }
  },
  actions: {
    editName2 ({ commit }, name) {
      setTimeout(() => {
        commit('GET_NAME2', name)
      }, 1000)
    }
  }
}
const store = new Vuex.Store({
  modules: {
    user,
    user2
  },
  getters: {
    name: state => state.user.name,
    name2: state => state.user2.name2
  }
})
```
```demo
<template>
  <div>
    <h2>{{ name }}</h2>
    <h2>{{ name2 }}</h2>
    <button @click="newName">获取新名字</button>
  </div>
</template>
<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  computed: {
    ...mapGetters(['name', 'name2'])
  },
  methods: {
    ...mapActions([
      'editName',
      'editName2'
    ]),
    newName () {
      this.editName('小明的第二个名字')
      this.editName2('小红的第二个名字')
    }
  }
}
</script>
```