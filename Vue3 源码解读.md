# Vue3 源码解读

## 项目目录结构

- packages
  - compiler-core 编译器核心
  - compiler-dom 处理浏览器dom相关的编译模块
  - compiler-sfc 单文件组件（.vue）文件的编译模块
  - compiler-ssr  服务端渲染的编译模块
  - reactivity 响应性系统的核心模块
  - reactivity-transform 已废弃
  - runtime-core 运行时核心代码
  - runtime-dom 基于浏览器的运行时
  - runtime-test 测试相关运行时
  - server-renderer 服务端渲染服务
  - sfc-playground sfc工具
  - shared 全局共享的工具
  - size-check 运行时包大小检查工具
  - template-explorer 线上测试
  - vue 包含了所有的测试实例以及打包后的dist文件
  - vue-compat vue3与vue2的兼容性代码与合并代码
- api-extractor.json api分析工具



## 运行测试实例，通过 debugger 观察 vue运行

## 开启 sourceMap

### 拓展 minimist

minimist 是nodejs的命令行参数解析工具







## rollup打包插件

```
pnpm install @rollup/plugin-commonjs --save-dev
pnpm install @rollup/plugin-typescript --save-dev
pnpm install @rollup/plugin-node-resolve --save-dev
```





## 路径映射





## Object.defineProperty 的缺陷

无法检测 数组和对象的变化。

getter 和 setter 是针对已经存在的属性的监听，所以在新增属性时是无法监听到的，所以新增的属性将失去响应性

监听指定对象的指定属性的 getter 和 setter







## effect 主要做了3件事情

- 生成 ReactiveEffect 实例
- 触发 fn 方法，从而激活 getter
- 建立了 targetMap 和 activeEffect 之间的联系
  - dep.add(activeEffect)
  - activeEffect.deps.push(dep)



## setter 主要做了2件事情

- 修改obj的值
- 触发 targetMap 下保存的 fn 函数



## reactive.html 实例解析

1. reactive 函数

2. effect 函数

3. obj.name = xxx 表达式

   ------

这三块代码代码背后的主要操作:

1. 创建 proxy
2. 收集 effect 的依赖
3. 触发收集的依赖





## 构建 reactive 函数，获取 proxy 实例





### 框架实现：什么是 WeakMap?它和Map 有什么区别？

- key 必须是对象
- key 是弱引用的

**弱引用**

弱引用：不会影响垃圾回收机制。即：WeakMap的key 不再存在任何引用时，会被直接回收。

强引用：会影响垃圾回收机制。存在强应用的对象永远不会被回收。



### what is reactivity

所谓的响应性指的就是：当响应性数据 触发  **setter** 时执行 fn 函数

那么想要达到这样的一个目的，就必须要在：**getter** 时收集到相关的 fn 函数，以便在 setter 的时候可以执行对应的 fn 函数

但是对于收集而言，如果仅仅是把 fn 存起来还是不够的，还需要知道，当前的这个 fn 是哪个响应式数据对象的哪个属性对应的，只有这样，才能在 具体的属性 触发 setter 的时候，准确的执行相关 fn 。

因此依赖收集的对象数据格式为：

Weakmap：

​	1、key：响应性对象（reactive的返回值）例如：const obj = reactive({name: 'xxx'})

​	2、value：Map对象

​		1、key：响应性对象的指定属性 (name)

​		2、指定对象的指定属性的 执行函数







## 构建 ref 函数

### ref 函数是如何进行实现的？



### ref 可以构建简单数据类型的响应性吗？



### 为什么 ref 类型的数据，必须要通过 .value 访问值呢？

