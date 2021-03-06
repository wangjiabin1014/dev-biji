# vue 长夜将至，我从今开始守望，至死方休

1. vue、react、angular三者有什么区别？
    - vue借鉴了angular的模板和数据绑定技术，又借鉴了react的组件化和虚拟DOM技术。
2. 谈谈你对vue的理解。
    - vue是一套用于构建用户界面的渐进式框架。（异步获取后台数据然后在页面上展示）应用的是MVVM模式，Vue被设计为可以自底向上逐层应用。Vue的核心库只关注视图层，不仅易上手还方便与第三方库或既有项目整合。
3. MVVM架构
   - 全称：Model-View-ViewModel，是一种软件架构模式。Model表示数据模型层，View表示视图层。而ViewModel则是二者的桥梁，从而实现数据的双向绑定
4. Vue的响应式数据原理
   - 发布-订阅模式，当一个对象（发布者）的状态发生改变时，所有依赖他的对象（订阅者）都会得到通知。
   - 通过数据劫持：```Object.defineProperty()```来劫持各个属性的getter()、setter()，在数据变动时就会触发相应的方法。
5. Vue中是如何检测数组变化的？
   - 数组就是使用```Object.defineProperty```来重新定义数组的原生方法：pop(), push(), shift(), unshift(), splice(), sort(), reverse()这七种方法。只要这些方法执行改变了数组内容，就可以检测了。
   1. 是用函数劫持的方式来重写数组方法，更改数组的原型改成自己的，用户调用数组的一些方法的时候走的就是自己的方法，然后通知视图去更新。
   2. 数组里的每一项可能是对象，那么就对数组的每一项进行观测，（且只有数组里的对象才能进行观测，观测过的数据便不会再进行观测了）。
   3. Vue3.0则改用了```proxy```，可以直接监听对象数组的变化。
6. 为什么Vue采用异步渲染？
   - Vue是组件级更新，如果不采用异步更新，那么每次数据更新都会对当前组件对行重新渲染。所以为了性能，Vue会在本轮数据更新后，会异步更新视图。核心思想```nextTick```
7. 了解nextTick吗
   - 异步方法，异步渲染最后异步，与JS事件循环紧密联系。主要使用了宏任务微任务。（```setTimeout()  ,  promise```那些），定义了一个异步方法。多次调用nextTick会将方法存入队列，然后通过异步方法来清空队列。
8. Vue的生命周期与钩子函数
    1. 钩子函数：
        - beforeCreat:实例初始化之后，数据观测之前调用
        - created：实例创建完之后调用。实例完成：数据观测、属性和方法的运算、watch/event事件回调。无$el。
        - beforeMount：在挂载之前调用，render函数首次被调用。
        - mounted：挂载完成，被新创建的vm.$el替换，并且挂载到实例上之后首次调用该函数。
        - beforeUpdate：在数据更新前调用，发生在虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。
        - updated：由于数据更新导致虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。
        - beforeDestroy：在实例销毁前调用，实例仍然可以用。
        - destroyed：实例销毁之后调用，调用之后，Vue实例指向的所有东西都会解绑，所有事件监听器和所有子实例都会被移除。
    2. 在生命周期内可以做什么
        - created：实例已经创建完成，因为他是最早触发的，所以可以进行一些数据、资源的请求。
        - mounted：实例挂载完毕，可以进行一些DOM的操作。
        - beforeUpdate：可以在这个函数中进一步更改状态，但是不会触发重新渲染。
        - updated：可以执行依赖于DOM的操作，但是要避免更改状态，因为可能会导致更新无限循环。
        - destroyed：可以执行一些优化操作，清空计时器，解除绑定事件。
    3. ajax放在哪个生命周期？
       -  一般是放在mounted中，来保证逻辑的统一性。因为生命周期是同步执行的而ajax是异步的。单数服务端渲染ssr统一放在created中，因为服务端渲染不支持mounted方法。
    4. 什么时候使用beforeDestroy？
       -  当前页面使用```$on```，需要解绑事件，清除定时器，```scroll mousemove```
9. 父子组件生命周期调用顺序
    - 渲染顺序：先父后子；完成顺序：先子后父
    - 更新顺序：父更新导致子更新
    - 销毁顺序：先父后子；完成顺序：先子后父
10. **VUE中使用```$emit(eventName)```触发事件，使用```$on(eventName)```监听事件**
11. **```$emit(eventName)```触发当前实例上的事件，附加参数都会传给监听器回调**
12. **```$on(eventName)```监听当前实例上的自定义事件，事件可以由vm.$emit触发。回调函数会接收所有传入事件触发函数的额外参数**
13. VUE组件通信
    - 父子间通信：父亲提供数据通过属性props传给儿子；儿子通过```$on```来绑定父亲的事件，再通过```$emit```触发自己的事件（发布订阅）。
    - 获取父子组件实例的方法：
      - 父组件提供数据，子组件注入。provide，inject，插件用的多。
      - ref获取组件实例，调用组件的属性、方法。
      - 跨组件通信```Event bus (Vue.prototype.bus = new Vue)```其实是基于```$on和$emit```
    - VUEX状态管理实现通信
14. Vuex工作原理
    1.  Vuex是专门为Vue.js应用程序开发的状态管理模式。
    2.  状态自管理应用包含以下几个部分：
        - state：驱动应用的数据源
        - view: 以声明方式将state映射到视图
        - actions：响应在view上的用户输入导致的状态变化。 
    3. Vuex，多组件共享状态，因为单向数据流的简洁性很容易被破坏：
       - 多个视图依赖于同一状态。
       - 来自不同视图的行为需要变更同一状态。
15. VNode(虚拟节点)：虚拟节点就是用一个对象来描述一个真实的DOM元素。 