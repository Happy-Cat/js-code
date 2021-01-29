## vue3.x

### 响应式API 

**vue2.x使用Object.defineProperty 进行数据劫持，劫持要在数据创建开始时就进行层层递归对数据进行绑定。Object.defineProperty只能实现对对象的属性进行劫持。所以对于对象上的方法或者新增、删除的属性则无能为力，为了劫持数组的变化，vue重写了数组的push，shift等方法，**

**vue3.x中则使用proxy对数据进行劫持，Proxy可以在用到数据的时候再进行对下一层属性的劫持，Proxy可以实现对整个对象的劫持，而且Proxy作为es6新属性，会得到浏览器更好的支持**

**vue3 把这个功能彻底独立成一个单独的包 @vue/reactivity，提供了reactive、effect、computed等方法，其中reactive用于定义响应式的数据，effect相当于是Vue2中的watcher，computed用于定义计算属性。**

### 组合式API

**Vue2.x页面将数据定义在data，computed层，逻辑处理放在methods,watch部分，如果页面特别复杂，而且没有进行组件抽离，就会导致逻辑处理列表变的非常的长，会导致功能难以阅读和理解，尤其对从未接触相关功能的人来说，vue3.x开始定义组合式api对同一个逻辑相关的代码进行组合，抽离成函数在页面中引入使用，这样使得功能点进行了分离，可以是代码的阅读性更强，方便理解，核心功能setup组件**

**Ref 我们可以通过一个新的 ref 函数使任何响应式变量在任何地方起作用**

**setup(props, { attrs, slots, emit })**

**setup组件， setup 选项是一个接受 props 和 context 的函数**

**props一般为组件之间的传递参数，props是响应式的，所以不能使用es6解构，vue 提供了toRefs方法来对props进行解构，如果没有定义对应的属性，需要使用type = toRef(props, type)来定义一个ref**

**{ attrs, slots, emit } = context attrs为vue提供对孙子组件传参提供的优化，slots为组件插件，emit子组件回馈父组件的方法**

**执行 setup 时，组件实例尚未被创建。因此，你只能访问 props、attrs、slots、emit，不能访问data、computed、methods**

**如果 setup 返回一个对象，则可以在组件的模板中像传递给 setup 的 props property 一样访问该对象的 property**

**在 setup () 内部调用生命周期钩子：onBeforeMount、onMounted、onBeforeUpdate、onUpdated、onBeforeUnmount、onUnmounted、onErrorCaptured、onRenderTracked、onRenderTriggered，包含了vue整个生命周期的所有钩子，说明这些生命周期中的所方法都可以setup实现**
