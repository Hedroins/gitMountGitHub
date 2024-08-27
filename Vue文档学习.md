1、createApp接受一个组件对象作为参数。

2、Vue 模板语法表达式，可以访问的全局对象受限，能访问到的全局对象包括：Math、Date等。可以通过app.config.globalProperties设置需要暴露的全局属性。

3、Vue指令的作用是在其表达式的值变化时响应式地更新 DOM（指令和Dom有关）

4、Vue动态参数<a :[attributeName]="url"> ... </a>

5、当你修改了响应式状态时，DOM 会被自动更新。但是需要注意的是，DOM 更新不是同步的。Vue 会在“next tick”更新周期中缓冲所有状态的修改，以确保不管你进行了多少次状态修改，每个组件都只会被更新一次。（改几次，最终触发渲染一次）

6、reactive可以将普通对象转为响应式对象，ref的值为对象时也会在内部调用reactive进行转换。对一个响应式对象赋值一个属性为对象，这个属性也会被转换为响应式对象。

7、reactive的参数只能是对象，不能是string、number 或 boolean 这样的原始类型。(reactive只能包装对象)

8、一个 ref 会在作为响应式对象的属性被访问或修改时自动解包。例如：
    const count = ref(0)
    const state = reactive({
      count
    })

    console.log(state.count) // 0

    state.count = 1
    console.log(count.value) 


9、如果将一个新的 ref 赋值给一个关联了已有 ref 的属性，那么它会替换掉旧的 ref：
     const otherCount = ref(2)
     state.count = otherCount
     console.log(state.count) // 2
       // 原始 ref 现在已经和 state.count 失去联系
     console.log(count.value) // 1
    只有当嵌套在一个深层响应式对象内时，才会发生 ref 解包。当其作为浅层响应式对象的属性被访问时不会解包。


10、当 ref 作为响应式数组或原生集合类型 (如 Map) 中的元素被访问时，它不会被解包：
    const books = reactive([ref('Vue 3 Guide')])
    // 这里需要 .value
    console.log(books[0].value)

    const map = reactive(new Map([['count', ref(0)]]))
    // 这里需要 .value
    console.log(map.get('count').value)


11、在模板渲染上下文中，只有顶级的 ref 属性才会被解包。也就是说，嵌套的 ref 属性不会解包。


12、如果 ref 是文本插值的最终计算值，那么它将被解包。
    let object = {id:ref(1)}
    {{ object.id }}会正常工作。 {{ object.id + 1 }}不能正常工作。


13、计算属性computed()期望接受一个getter函数作为参数，返回一个只读的响应式ref对象。


14、在 Vue 3 中，你可以使用 shallowRef() 来创建一个浅层的 ref 对象。


15、在 Vue 3 中，你可以使用 shallowReactive() 来创建一个浅层的响应式对象。


16、在 Vue 3 中，你可以使用 shallowReadonly() 来创建一个浅层的只读响应式对象。


17、计算属性和方法的区别在于，计算属性值会基于其响应式依赖被缓存。一个计算属性仅会在其响应式依赖更新时才重新计算。而 方法总是会在每次重新渲染时进行调用。


18、在 Vue 3 中，你可以使用 readonly() 来创建一个只读的响应式对象或只读的 ref 对象。


19、在 Vue 3 中，你可以使用 toRef() 来创建一个 ref 对象，该 ref 对象指向原始对象中的一个属性。


20、在 Vue 3 中，你可以使用 toRefs() 来创建一个包含原始对象中所有属性 ref 对象的响应式对象。

21、计算属性默认是只读的，只在某些特殊场景中你可能才需要用到“可写”的属性，你可以通过同时提供 getter 和 setter 来创建。 例如：
    import { ref, computed } from 'vue'
    const firstName = ref('John')
    const lastName = ref('Doe')

    const fullName = computed({
      // getter
    get() {
         return firstName.value + ' ' + lastName.value
     },
     // setter
    set(newValue) {
       // 注意：我们这里使用的是解构赋值语法
       [firstName.value, lastName.value] = newValue.split(' ')
     }
   })

### 22、v-if 是“真实的”按条件渲染，因为它确保了在切换时，条件区块内的事件监听器和子组件都会被销毁与重建
### v-if 也是惰性的：如果在初次渲染时条件值为 false，则不会做任何事。条件区块只有当条件首次变为 true 时才被渲染。
### v-show 简单许多，元素无论初始条件如何，始终会被渲染，只有 CSS display 属性会被切换。
### v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销


23、当 v-if 和 v-for 同时存在于一个元素上的时候，v-if 会首先被执行。

24、可以在定义 v-for 的变量别名时使用解构，和解构函数参数类似：
     <li v-for="{ message } in items">
       {{ message }}
    </li>

<!-- 有 index 索引时 -->
      <li v-for="({ message }, index) in items">
       {{ message }} {{ index }}
     </li>

25、v-for中in 可以用of替换。

26、你也可以使用 v-for 来遍历一个对象的所有属性。遍历的顺序会基于对该对象调用 Object.keys() 的返回值来决定。
<li v-for="(value, key, index) in myObject"> // value值 ， key键， index索引
  {{ index }}. {{ key }}: {{ value }}
</li>


27、v-for 可以直接接受一个整数值

28、可以在 <template> 标签上使用 v-for 来渲染一个包含多个元素的块。

29、当它们同时存在于一个节点上时，v-if 比 v-for 的优先级更高。这意味着 v-if 的条件将无法访问到 v-for 作用域内定义的变量别名。

### 30、Vue 默认按照“就地更新”的策略来更新通过 v-for 渲染的元素列表。当数据项的顺序改变时，Vue 不会随之移动 DOM 元素的顺序，而是就地更新每个元素，确保它们在原本指定的索引位置上渲染。适用于列表渲染输出的结果不依赖子组件状态或者临时 DOM 状态 ，为了给 Vue 一个提示，以便它可以跟踪每个节点的标识，从而重用和重新排序现有的元素，你需要为每个元素对应的块提供一个唯一的 key attribute：key 绑定的值期望是一个基础类型的值，例如字符串或 number 类型。不要用对象作为 v-for 的 key（因为Vue默认会就地更新，所以给它一个Key,起重用的目的）。


31、数值的变更方法，push() pop() shift() unshift() splice() sort() reverse() Vue 能够侦听响应式数组的变更方法，并在它们被调用时触发相关的更新。

32、有时，我们希望显示数组经过过滤或排序后的内容，而不实际变更或重置原始数据。在这种情况下，你可以创建返回已过滤或已排序数组的计算属性。（计算属性应用之一，避免修改源数据）

33、系统按键修饰符和常规按键不同。与 keyup 事件一起使用时，该按键必须在事件发出时处于按下状态。换句话说，keyup.ctrl 只会在你仍然按住 ctrl 但松开了另一个键时被触发。若你单独松开 ctrl 键将不会触发。（系统按键修饰符的特殊性）


34、v-model可以用于input，textarea和select元素，它会根据控件类型自动选取正确的方法来更新元素。


35、v-model在内部为不同的输入元素使用不同的属性并抛出不同的事件：
     text 和 textarea 元素使用 value 属性和 input 事件；
     checkbox 和 radio 使用 checked 属性和 change 事件；
     select 字段将 value 作为 prop 并将 change 作为事件。


36、<input type="checkbox" v-model="toggle" true-value="yes" false-value="no" /> true-value 和 false-value 是 Vue 特有的 attributes，仅支持和 v-model 配套使用。这里 toggle 属性的值会在选中时被设为 'yes'，取消选择时设为 'no'


37、v-model修饰符，trim , number , lazy

38、watch 常规用法 
const question = ref('')
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.indexOf('?') > -1) {
    try {
      const res = await fetch('https://yesno.wtf/api')
      question.value = (await res.json()).answer
    } catch (error) {
      question.value = 'Error! Could not reach the API. ' + error
    }
  }
})
watch 的第一个参数可以是不同形式的“数据源”：它可以是一个 ref (包括计算属性)、一个响应式对象、一个 getter 函数、或多个数据源组成的数组。（watch第一个参数支持三种类型的数据加上三种类型的数组的组合）



39、watch 深度监听
  ###  1)直接给 watch() 传入一个响应式对象，会隐式地创建一个深层侦听器——该回调函数在所有嵌套的变更时都会被触发
  ###  2）一个返回响应式对象的 getter 函数，只有在返回不同的对象时，才会触发回调
  ###  3）可以给2显式地加上 deep 选项(watch第三个参数传入对象{deep:true})，强制转成深层侦听器：


40、watch 监听多个数据源,
watch([fooRef, barRef], ([newFoo, newBar], [prevFoo, prevBar]) => {
  console.log(`fooRef changed: ${prevFoo} => ${newFoo}`)
  console.log(`barRef changed: ${prevBar} => ${newBar}`)
})
fooRef 和 barRef可以是响应式对象，也可以是 getter 函数。


41、watch 监听对象，不设置deep属性，则是监听整个对象替换则触发执行。


42、watch 监听对象属性 ，你不能直接侦听响应式对象的属性值，例如:
    const obj = reactive({ count: 0 })

// 错误，因为 watch() 得到的参数是一个 number
watch(obj.count, (count) => {
  console.log(`Count is: ${count}`)
})

// 提供一个 getter 函数
watch(
  () => obj.count,
  (count) => {
    console.log(`Count is: ${count}`)
  }
)


43、watch 监听数组与监听对象类似。


44、即时回调监听器，可以通过传入 immediate: true 选项来强制侦听器的回调立即执行：
watch(
  source,
  (newValue, oldValue) => {
    // 立即执行，且当 `source` 改变时再次执行
  },
  { immediate: true }   //{once:true}是一次性监听器，只触发一次
)

45、watchEffect 简化watch 当监听源和回调中使用相同的响应式数据，则可以使用watchEffect更方便。


46、watch回调的触发时机，侦听器回调会在父组件更新 (如有) 之后、所属组件的 DOM 更新之前被调用。这意味着如果你尝试在侦听器回调中访问所属组件的 DOM，那么 DOM 将处于更新前的状态（watch的更新默认更新时机，在父组件更新之后，所属组件Dom更新之前，所以watch回调中默认访问的是组件Dom更新之前的状态）

47、如果想在侦听器回调中能访问被 Vue 更新之后的所属组件的 DOM，你需要指明 flush: 'post'
watch(source, callback, {
  flush: 'post'
})

watchEffect(callback, {
  flush: 'post'
})
后置刷新的 watchEffect() 有个更方便的别名 watchPostEffect()


48、你还可以创建一个同步触发的侦听器，它会在 Vue 进行任何更新之前触发：
同步触发的 watchEffect() 有个更方便的别名 watchSyncEffect()：
import { watchSyncEffect } from 'vue'

watchSyncEffect(() => {
  /* 在响应式数据变化时同步执行 */
})
#### 同步侦听器不会进行批处理，每当检测到响应式数据发生变化时就会触发。可以使用它来监视简单的布尔值，但应避免在可能多次同步修改的数据源 (如数组) 上使用。


49、watchEffect 仅会在其同步执行期间，才追踪依赖。在使用异步回调时，只有在第一个 await 正常工作前访问到的属性才会被追踪。（watchEffect排斥异步化，只有在异步调用之前的依赖才会被追踪）


50、要手动停止一个侦听器，请调用 watch 或 watchEffect 返回的函数，异步创建的侦听器需要手动停止。同步不需要（同步会绑定到当前的组件实例对象中，会根据组件的销毁而销毁）
const unwatch = watchEffect(() => {})

// ...当该侦听器不再需要时
unwatch()


51、watch和watchEffect的区别：
     watch之后侦听明确追踪的数据源，不会追踪回调中访问的响应式数据。watchEffect会追踪回调中访问的响应式数据，并在数据源变化时重新运行回调。
     watch会避免在副作用发生时追踪依赖，因此可以精确控制回调函数的触发时机。watchEffect会在副作用发生期间追踪依赖。
     watch默认不会立即执行，而watchEffect会默认执行立即执行。


52、watchEffect与watch相比的优势：
      1）对于有多个依赖项的侦听器来说，使用 watchEffect() 可以消除手动维护依赖列表的负担
      2）如果你需要侦听一个嵌套数据结构中的几个属性，watchEffect() 可能会比深度侦听器更有效，
          因为它将只跟踪回调中被使用到的属性，而不是递归地跟踪所有的属性。



53、模版引用ref ，它允许我们在一个特定的 DOM 元素或子组件实例被挂载后，获得对它的直接引用。
    // 声明一个 ref 来存放该元素的引用
    // 必须和模板里的 ref 同名
     const input = ref(null)

    如果你需要侦听一个模板引用 ref 的变化，确保考虑到其值为 null 的情况：
    watchEffect(() => {
     if (input.value) {
        input.value.focus()
      } else {
      // 此时还未挂载，或此元素已经被卸载（例如通过 v-if 控制）
     }
    })


    54、在v-for上使用模版引用，对应的 ref 中包含的值是一个数组，它将在元素被挂载后包含对应整个列表的所有元素：

    55、函数模版引用：
  ### ref attribute 还可以绑定为一个函数，会在每次组件更新时都被调用。该函数会收到元素引用作为其第一个参数：
         <input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 */ }">
  ### 当绑定的元素被卸载时，函数也会被调用一次，此时的 el 参数会是 null。

       
   56、对于使用了<script setup> 的组件，其方式和属性都是私有的，因此无法在模版引用中直接访问它们，需要子组件使用defineExpose() 宏来显式地声明暴露给父组件的属性：
         <script setup>
         import { ref, defineExpose } from 'vue'

         const myRef = ref(null)

         // 使 myRef 在模板中可用
         defineExpose({
           myRef
         })
         </script>


57、当不使用构建步骤时，一个 Vue 组件以一个包含 Vue 特定选项的 JavaScript 对象来定义。


58、defineProps 是一个仅 <script setup> 中可用的编译宏命令，并不需要显式地导入。
声明的 props 会自动暴露给模板。defineProps 会返回一个对象，其中包含了可以传递给组件的所有 props

如果你没有使用 <script setup>，props 必须以 props 选项的方式声明，props 对象会作为 setup() 函数的第一个参数被传入：
export default {
  props: ['title'],
  setup(props) {
    console.log(props.title)
  }
}

59、defineEmits 仅可用于 <script setup> 之中，并且不需要导入，它返回一个等同于 $emit 方法的 emit 函数。
它可以被用于在组件的 <script setup> 中抛出事件，因为此处无法直接访问 $emit：
<script setup>
const emit = defineEmits(['enlarge-text'])

emit('enlarge-text')
</script>

60、如果你没有在使用 <script setup>，你可以通过 emits 选项定义组件会抛出的事件。
你可以从 setup() 函数的第二个参数，即 setup 上下文对象上访问到 emit 函数：
export default {
  emits: ['enlarge-text'],
  setup(props, ctx) {
    ctx.emit('enlarge-text')
  }
}

61、我们可以使用 Vue 应用实例的 .component() 方法，让组件在当前 Vue 应用中全局可用。component() 方法可以被链式调用。
import { createApp } from 'vue'

const app = createApp({})

app.component(
  // 注册的名字
  'MyComponent',
  // 组件的实现
  {
    /* ... */
  }
)

62、全局注册的缺点：
   1、全局注册，但并没有被使用的组件无法在生产打包时被自动移除 (也叫“tree-shaking”)。
   2、全局注册在大型项目中使项目的依赖关系变得不那么明确。在父组件中使用子组件时，不太容易定位子组件的实现。
   和使用过多的全局变量一样，这可能会影响应用长期的可维护性。


63、对于以对象形式声明的每个属性，key 是 prop 的名称，而值则是该 prop 预期类型的构造函数。
// 使用 <script setup>
defineProps({
  title: String,
  likes: Number
})


64、如果你想要将一个对象的所有属性都当作 props 传入，你可以使用没有参数的 v-bind，即只使用 v-bind 而非 :prop-name。例如，这里有一个 post 对象
const post = {
  id: 1,
  title: 'My Journey with Vue'
}
以及下面的模板：
<BlogPost v-bind="post" />
而这实际上等价于：
<BlogPost :id="post.id" :title="post.title" />


65、当对象或数组作为 props 被传入时，虽然子组件无法更改 props 绑定，但仍然可以更改对象或数组内部的值。
这是因为 JavaScript 的对象和数组是按引用传递，对 Vue 来说，阻止这种更改需要付出的代价异常昂贵。


66、props校验 url[https://cn.vuejs.org/guide/components/props.html#Prop%E6%A0%A1%E9%AA%8C]

67、props的type原生类型可以是以下类型String、Number、Boolean、Array、Object、Date、Function、Symbol、Error

68、type 也可以是自定义的类或构造函数，Vue 将会通过 instanceof 来检查类型是否匹配。

69、如果该类型是必传的但可为空，你可以用一个包含 null 的数组语法：
defineProps({
  id: {
    type: [String, null],
    required: true
  }
})
如果仅为null则会允许任何类型。


70、<!-- 等同于传入 :disabled="true" -->
<MyComponent disabled /> 特殊情况Boolean和String同时存在，在Boolean声明在String之前有效


71、如果一个原生事件的名字 (例如 click) 被定义在 emits 选项中，则监听器只会监听组件触发的 click 事件而不会再响应原生的 click 事件。

72、事件校验；要为事件添加校验，那么事件可以被赋值为一个函数，接受的参数就是抛出事件时传入 emit 的内容，返回一个布尔值来表明事件是否合法。
<script setup>
const emit = defineEmits({
  // 没有校验
  click: null,

  // 校验 submit 事件
  submit: ({ email, password }) => {
    if (email && password) {
      return true
    } else {
      console.warn('Invalid submit event payload!')
      return false
    }
  }
})

function submitForm(email, password) {
  emit('submit', { email, password })
}
</script>


73、组件v-model,
父组件：
<UserName
  v-model:first-name="first"
  v-model:last-name="last"
/>

子组件：
<script setup>
const firstName = defineModel('firstName') //firstName为一个ref
const lastName = defineModel('lastName')
</script>

<template>
  <input type="text" v-model="firstName" />
  <input type="text" v-model="lastName" />
</template>

此时改变子组件的firstName，父组件会同步变化。自定义v-model修饰符详见：url[https://cn.vuejs.org/guide/components/v-model.html]


74、你可以在 <script setup> 中使用 useAttrs() API 来访问一个组件的所有透传 attribute：
<script setup>
import { useAttrs } from 'vue'

const attrs = useAttrs()
</script>


75、插槽的默认内容，在slot标签之间写默认需要展示的内容


76、插槽的具名插槽，在slot标签上添加name属性，在父组件中使用v-slot:name来指定插槽，v-slot简写为#

77、当具名插槽和默认插槽同时存在时，所有顶级的非template节点都会被视为默认插槽的内容。

78、条件插槽，有时你需要根据插槽是否存在来渲染某些内容。你可以结合使用 $slots 属性与 v-if 来实现。
<template>
  <div class="card">
    <div v-if="$slots.header" class="card-header">
      <slot name="header" />
    </div>
    
    <div v-if="$slots.default" class="card-content">
      <slot />
    </div>
    
    <div v-if="$slots.footer" class="card-footer">
      <slot name="footer" />
    </div>
  </div>
</template>

79、动态插槽名，在template标签上使用v-slot:[name]来指定插槽，v-slot简写为#
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>

  <!-- 缩写为 -->
  <template #[dynamicSlotName]>
    ...
  </template>
</base-layout>


80、作用域插槽 可以像对组件传递 props 那样，向一个插槽的出口上传递 attributes：
子组件：
<!-- <MyComponent> 的模板 -->
<div>
  <slot :text="greetingMessage" :count="1"></slot>
</div>
通过子组件标签上的 v-slot 指令，直接接收到了一个插槽 props 对象：
父组件：
<MyComponent v-slot="slotProps"> // 也可以直接解构v-slot="{ text, count }"
  {{ slotProps.text }} {{ slotProps.count }}
</MyComponent>

具名作用域插槽的工作方式也是类似的，插槽 props 可以作为 v-slot 指令的值被访问到：v-slot:name="slotProps"
使用缩写时是这样的：
<MyComponent>
  <template #header="headerProps">
    {{ headerProps }}
  </template>

  <template #default="defaultProps">
    {{ defaultProps }}
  </template>

  <template #footer="footerProps">
    {{ footerProps }}
  </template>
</MyComponent>
如果默认插槽和具名插槽同时使用，则需要为默认插槽使用显式的 <template> 标签  url[https://cn.vuejs.org/guide/components/slots.html]


81、依赖注入，
   父组件：provide('message','hello') //第一个参数可以是字符串或者Symbol，第二个参数可以是任意类型包括ref
        
   子组件：const message = inject('message') // 如果提供的值是一个ref，注入的值是一个ref对象并不会自动解包。在注入时，如果没有任何一个
          //祖先组件提供。可以传入默认值，inject('message','默认值')

   除了在组件层面提供依赖注入，也可以在整个应用层面提供：

      const app = createApp(App);
      app.provide('message','hello')


82、和响应式数据配合使用：如果提供的是一个响应式数据，那么包括更改数据的方法最好也在提供的组件中定义，然后暴露给注入组件。
url[https://cn.vuejs.org/guide/components/provide-inject.html]
如果不想注入组件修改提供的值，可以用readonly保证提供的值。例如：
let message = ref('hello')
provide('message',readonly(message))


83、异步组件，在大型项目中，我们可能需要拆分应用为更小的块，并仅在需要时再从服务器加载相关组件。Vue 提供了 defineAsyncComponent 方法来实现此功能：
defineAsyncComponent接受一个返回promise的加载函数，这个Promise的resolve方法应该在从服务器获取到组件定义时调用。Promise的reject在获取组件失败时调用。
ES的动态模块导入也返回一个Promise因此通常与defineAsyncComponent配合使用。
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)

返回的AsyncComp是一个经过外层包装过的组件，只会在需要是渲染组件，组件的上绑定的props以及插槽中的内容都会传给内部组件。

异步组件可以全局注册：
app.component('Comp',defineAsyncComponent(()=>import('./component/Comp.vue')))
也可以在父组件中定义



83、异步组件高级选项：

const AsyncComp = defineAsyncComponent({
  loader: import('./component/Comp.vue'), //加载函数
  loadingComponent:loadingComponent, //加载组件时使用的组件
  errorComponent:errorComponent,  //加载错误时使用的组件
  delay:200,//加载组件前的延时时间
  timeout:300,//超时时间。如果配置了，并且超时则会展示errorComponent组件
})


84、异步组件搭配Suspense内部组件使用


85、组合式函数，“组合式函数”(Composables) 是一个利用 Vue 的组合式 API 来封装和复用有状态逻辑的函数。
    对于组合式函数中的响应式数据，会在不同的组件实例中创建不同值的拷贝，彼此直接不会产生影响。如果要共享状态则需要用到状态管理工具，如pinia
    详见url[https://cn.vuejs.org/guide/reusability/composables.html]

89、自定义指令主要是为了重用涉及普通元素的底层 DOM 访问的逻辑。
   一个自定义指令由一个包含类似组件生命周期钩子的对象来定义。钩子函数会接收到指令所绑定元素作为其参数。
   在 <script setup> 中，任何以 v 开头的驼峰式命名的变量都可以被用作一个自定义指令
   在没有使用 <script setup> 的情况下，自定义指令需要通过 directives 选项注册：

   将一个自定义指令全局注册到应用层级也是一种常见的做法：

const app = createApp({})

// 使 v-focus 在所有组件中都可用
app.directive('focus', {
  /* ... */
})

一个指令的定义对象可以提供7种钩子函数 (都是可选的) 和组件的生命周期钩子函数一样，除了beforeCreate,
每个钩子函数接受4个参数

 created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
    //el：指令绑定到的元素。这可以用于直接操作 DOM。
    //binding：一个对象，包含以下属性。
     //value：传递给指令的值。例如在 v-my-directive="1 + 1" 中，值是 2。
     // oldValue：之前的值，仅在 beforeUpdate 和 updated 中可用。无论值是否更改，它都可用。
     //   arg：传递给指令的参数 (如果有的话)。例如在 v-my-directive:foo 中，参数是 "foo"。
     //modifiers：一个包含修饰符的对象 (如果有的话)。例如在 v-my-directive.foo.bar 中，修饰符对象是 { foo: true, bar: true }。
     //instance：使用该指令的组件实例。
     //dir：指令的定义对象

  //vnode：代表绑定元素的底层 VNode。

  // prevVnode：代表之前的渲染中指令所绑定元素的 VNode。仅在 beforeUpdate 和 updated 钩子中可用。
  },
除了 el 外，其他参数都是只读的，不要更改它们。若你需要在不同的钩子间共享信息，推荐通过元素的 dataset attribute 实现

简化形式：仅仅需要在 mounted 和 updated 上实现相同的行为，除此之外并不需要其他钩子。这种情况下我们可以直接用一个函数来定义指令，如下所示：

<div v-color="color"></div>

app.directive('color', (el, binding) => {
  // 这会在 `mounted` 和 `updated` 时都调用
  el.style.color = binding.value
})

url[https://cn.vuejs.org/guide/reusability/custom-directives.html]


自定义指令传递对象字面量：
<div v-demo="{ color: 'white', text: 'hello!' }"></div>
app.directive('demo', (el, binding) => {
  console.log(binding.value.color) // => "white"
  console.log(binding.value.text) // => "hello!"
})

自定义指令不推荐在组件中使用，如果是一个根元素的组件，和透传attribute类似，自定义指令会绑定根元素，如果是多个根元素，则会被忽略并抛出警告。


90、插件
插件通常用来为 Vue 添加全局功能。
一个插件可以是一个拥有 install() 方法的对象，也可以直接是一个安装函数本身。安装函数会接收到安装它的应用实例和传递给 app.use() 的额外选项作为参数：
const myPlugin = {
  install(app, options) {
    // 配置此应用
  }
}
插件实例详见url[https://cn.vuejs.org/guide/reusability/plugins.html]


91、Transition组件和TransitionGroup组件。
<Transition> 会在一个元素或组件进入和离开 DOM 时应用动画。本章节会介绍如何使用它。

<TransitionGroup> 会在一个 v-for 列表中的元素或组件被插入，移动，或移除时应用动画。我们将在下一章节中介绍。
<Transition> 仅支持单个元素或组件作为其插槽内容。如果内容是一个组件，这个组件必须仅有一个根元素。
transition组件的6个内置class：
v-enter-from
v-enter-active
v-enter-to
v-leave-from
v-leave-active
v-leave-to
8个js钩子函数：
before-enter
enter
after-enter
enter-cancelled
before-leave
leave
after-leave
leave-cancelled
url[https://cn.vuejs.org/guide/built-ins/transition.html#the-transition-component]


92、<TransitionGroup> 支持和 <Transition> 基本相同的 props、CSS 过渡 class 和 JavaScript 钩子监听器，但有以下几点区别：

默认情况下，它不会渲染一个容器元素。但你可以通过传入 tag prop 来指定一个元素作为容器元素来渲染。

过渡模式在这里不可用，因为我们不再是在互斥的元素之间进行切换。

列表中的每个元素都必须有一个独一无二的 key attribute。

CSS 过渡 class 会被应用在列表内的元素上，而不是容器元素上。

移动动画
.list-move, /* 对移动中的元素应用的过渡 */
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

url[https://cn.vuejs.org/guide/built-ins/transition-group.html]


93、<KeepAlive> 是一个内置组件，它的功能是在多个组件间动态切换时缓存被移除的组件实例。
<KeepAlive> 默认会缓存内部的所有组件实例，但我们可以通过 include 和 exclude prop 来定制该行为。
我们可以通过传入 max prop 来限制可被缓存的最大组件实例数。<KeepAlive> 的行为在指定了 max 后类似一个 LRU 缓存：
如果缓存的实例数量即将超过指定的那个最大数量，则最久没有被访问的缓存实例将被销毁，以便为新的实例腾出空间。
onActivated(() => {
  // 调用时机为首次挂载
  // 以及每次从缓存中被重新插入时
})

onDeactivated(() => {
  // 在从 DOM 上移除、进入缓存
  // 以及组件卸载时调用
})



url[https://cn.vuejs.org/guide/built-ins/keep-alive.html]


94、<Teleport> 是一个内置组件，它可以将一个组件内部的一部分模板“传送”到该组件的 DOM 结构外层的位置去。
<Teleport to="body">...</Teleport>
<Teleport to="#modals">...</Teleport>
<Teleport to="custom">...</Teleport>


95、<Suspense> 是一个内置组件，用来在组件树中协调对异步依赖的处理。它让我们可以在组件树上层等待下层的多个嵌套异步依赖项解析完成，并可以在等待时渲染一个加载状态
<Suspense>
  <template #default>
    <AsyncComponent />
  </template>
  <template #fallback>
    <div>Loading...</div>
  </template>
</Suspense>

详见url[https://cn.vuejs.org/guide/built-ins/suspense.html]

96、测试相关，url[https://cn.vuejs.org/guide/scaling-up/testing.html]

97、想要导航到不同的 URL，可以使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，会回到之前的 URL。

98、router.push({ path: '/home', replace: true }) 替换当前位置，// 相当于
router.replace({ path: '/home' })，它在导航时不会向 history 添加新记录，正如它的名字所暗示的那样——它取代了当前的条目。


99、横跨历史：
// 向前移动一条记录，与 router.forward() 相同
router.go(1)

// 返回一条记录，与 router.back() 相同
router.go(-1)

// 前进 3 条记录
router.go(3)

100、命名视图,在一个路由配置中设置多个命名视图，可以很方便地同时渲染多个视图。
<template>
<router-view></router-view>
<router-view name="a"></router-view>
<router-view name="b"></router-view>
</template>

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      components: {
        default: Home,
        a: About,
        b: B
      }
    }
  ]
})

101、重定向也是通过 routes 配置来完成，下面例子是从 /home 重定向到 /：
const routes = [{ path: '/home', redirect: '/' }]
重定向的目标也可以是一个命名的路由：
const routes = [{ path: '/home', redirect: { name: 'homepage' } }]
甚至是一个方法，动态返回重定向目标：
const routes = [
  {
    // /search/screens -> /search?q=screens
    path: '/search/:searchText',
    redirect: to => {
      // 方法接收目标路由作为参数
      // return 重定向的字符串路径/路径对象
      return { path: '/search', query: { q: to.params.searchText } }
    },
  },
  {
    path: '/search',
    // ...
  },
]
请注意，导航守卫并没有应用在跳转路由上，而仅仅应用在其目标上。在上面的例子中，在 /home 路由中添加 beforeEnter 守卫不会有任何效果。

102、路由别名。
const routes = [
  {
    path: '/users',
    component: UsersLayout,
    children: [
      // 为这 3 个 URL 呈现 UserList
      // - /users
      // - /users/list
      // - /people
      { path: '', component: UserList, alias: ['/people', 'list'] },
    ],
  },
]
url[https://router.vuejs.org/zh/guide/essentials/redirect-and-alias.html]


103、路由中，使用props传参，通过设置 props: true 来配置路由将 id 参数作为 prop 传递给组件：

const routes = [
  { path: '/user/:id', component: User, props: true }
]

<!-- User.vue -->
<script setup>
defineProps({
  id: String
})
</script>

<template>
  <div>
    User {{ id }}
  </div>
</template>

当 props 设置为 true 时，route.params 将被设置为组件的 props。
对于有命名视图的路由，你必须为每个命名视图定义 props 配置：
const routes = [
  {
    path: '/user/:id',
    components: { default: User, sidebar: Sidebar },
    props: { default: true, sidebar: false }
  }
]

当 props 是一个对象时，它将原样设置为组件 props。当 props 是静态的时候很有用。
const routes = [
  {
    path: '/promotion/from-newsletter',
    component: Promotion,
    props: { newsletterPopup: false }
  }
]

你可以创建一个返回 props 的函数。这允许你将参数转换为其他类型，将静态值与基于路由的值相结合等等。
const routes = [
  {
    path: '/search',
    component: SearchUser,
    props: route => ({ query: route.query.q })
  }
]

你还可以通过 <RouterView> 插槽 传递任意参数：

template
<RouterView v-slot="{ Component }">
  <component
    :is="Component"
    view-prop="value"
   />
</RouterView>


103、两个路由匹配时的样式类，.router-link-active 和 .router-link-exact-active。
区别是.router-link-active不是精准匹配，如果有嵌套路由，父路由也会匹配，.router-link-exact-active是精准匹配，只有匹配当前路由

RouterLink组件可以通过activeClass 和 exactActiveClass 两个props来配置.router-link-active 和 .router-link-exact-active的别名。

也可以在定义路由时全局配置：
const router = createRouter({
  linkActiveClass: 'border-indigo-500',
  linkExactActiveClass: 'border-indigo-700',
  // ...
})

105、hash模式它在内部传递的实际 URL 之前使用了一个哈希字符（#）。由于这部分 URL 从未被发送到服务器，所以它不需要在服务器层面上进行任何特殊处理。
106、memory模式不会与 URL 交互也不会自动触发初始导航。这使得它非常适合 Node 环境和 SSR。它是用 createMemoryHistory() 创建的，
并且需要你在调用 app.use(router) 之后手动 push 到初始导航。它不会有历史记录，这意味着你无法后退或前进。
107、html5模式使用这种历史模式时，URL 会看起来很 "正常"，例如 https://example.com/user/id，但是直接访问url会出现404报错，需要做的就是在你的服务器上添加一个简单的回退路由

108、vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。这里有很多方式植入路由导航中：全局的，单个路由独享的，或者组件级的。

109、全局前置守卫
const router = createRouter({ ... })

router.beforeEach((to, from, next) => {
  // return false 可以通过return false取消跳转，
  //return {name:'Login'} 会中断当前导航，并以相同的from创建新的导航，跳转到login页面，也可以传递replace:true，这样会替换当前的history记录
  // next() 可选，如果传入了必须被调用，否则钩子就不会被 resolved
})

to: Route: 即将要进入的目标 路由对象
from: Route: 当前导航正要离开的路由
next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

如果发生错误则调用router.onError()注册的回调函数


110、全局解析守卫，每次导航时都会触发，不同的是，解析守卫刚好会在导航被确认之前、所有组件内守卫和异步路由组件被解析之后调用
const router = createRouter({ ... })

router.beforeResolve((to, from, next) => {
  // ...
})

to: Route: 即将要进入的目标 路由对象
from: Route: 当前导航正要离开的路由
next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。


111、全局后置钩子
const router = createRouter({ ... })

router.afterEach((to, from) => {
  // ...
})

to: Route: 即将要进入的目标 路由对象
from: Route: 当前导航正要离开的路由


112、路由独享的守卫
const router = createRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})


113、组件内的守卫
const Foo = {
  template: `...`,
  beforeRouteEnter(to, from, next) {
  // 不能访问this，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建
}
  beforeRouteUpdate(to, from, next) {
  
  }
  beforeRouteLeave(to, from, next) {
  
  }
  }

114、完整的导航解析流程
导航被触发。
在失活的组件里调用 beforeRouteLeave 守卫。
调用全局的 beforeEach 守卫。
在重用的组件里调用 beforeRouteUpdate 守卫(2.2+)。
在路由配置里调用 beforeEnter。
解析异步路由组件。
在被激活的组件里调用 beforeRouteEnter。
调用全局的 beforeResolve 守卫(2.5+)。
导航被确认。
调用全局的 afterEach 钩子。
触发 DOM 更新。
调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。


115、路由元信息，定义路由的时候你可以这样配置 meta 字段：
称呼 routes 配置中的每个路由对象为 路由记录。路由记录可以是嵌套的，因此，当一个路由匹配成功后，它可能匹配多个路由记录。
例如，根据上面的路由配置，/posts/new 这个 URL 将会匹配父路由记录 (path: '/posts') 以及子路由记录 (path: 'new')。
一个路由匹配到的所有路由记录会暴露为 route 对象(还有在导航守卫中的路由对象)的route.matched 数组。我们需要遍历这个数组来检查路由记录中的 meta 字段，
是 Vue Router 还为你提供了一个 route.meta，它是一个非递归合并所有 meta 字段


116、RouterView插槽。
<router-view v-slot="{ Component }">
  <component :is="Component" some-prop="a value">
    <p>Some slotted content</p>
  </component>
</router-view>
使用插槽可以让我们直接将模板引用放置在路由组件上：
<router-view v-slot="{ Component }">
  <component :is="Component" ref="mainContent" />
</router-view>


117、基于路由的动态过渡
<!-- 使用动态过渡名称 -->
<router-view v-slot="{ Component, route }">
  <transition :name="route.meta.transition">
    <component :is="Component" />
  </transition>
</router-view>
我们可以添加一个 after navigation hook，根据路径的深度动态添加信息到 meta 字段。
router.afterEach((to, from) => {
  const toDepth = to.path.split('/').length
  const fromDepth = from.path.split('/').length
  to.meta.transition = toDepth < fromDepth ? 'slide-right' : 'slide-left'
})


118、路由懒加载：意味着你可以用动态导入代替静态导入
component (和 components) 配置接收一个返回 Promise 组件的函数，Vue Router 只会在第一次进入页面时才会获取这个函数，
然后使用缓存数据。这意味着你也可以使用更复杂的函数，只要它们返回一个 Promise
// 将
// import UserDetails from './views/UserDetails.vue'
// 替换成
const UserDetails = () => import('./views/UserDetails.vue')

const router = createRouter({
  // ...
  routes: [
    { path: '/users/:id', component: UserDetails }
    // 或在路由定义里直接使用它
    { path: '/users/:id', component: () => import('./views/UserDetails.vue') },
  ],
})

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用命名 chunk，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)：

const UserDetails = () =>
  import(/* webpackChunkName: "group-user" */ './UserDetails.vue')
const UserDashboard = () =>
  import(/* webpackChunkName: "group-user" */ './UserDashboard.vue')
const UserProfileEdit = () =>
  import(/* webpackChunkName: "group-user" */ './UserProfileEdit.vue')
webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。


119、监测导航故障：
如果导航被阻止，导致用户停留在同一个页面上，由 router.push 返回的 Promise 的解析值将是 Navigation Failure。否则，它将是一个 falsy 值(通常是 undefined)。这样我们就可以区分我们导航是否离开了当前位置：
const navigationResult = await router.push('/my-profile')

if (navigationResult) {
  // 导航被阻止
} else {
  // 导航成功 (包括重新导航的情况)
  this.isMenuOpen = false
}

isNavigationFailure方法可以鉴别是否是导航故障
url[outer.vuejs.org/zh/guide/advanced/navigation-failures.htm]



120、滚动行为
const router = createRouter({
  history: createWebHashHistory(),
  routes: [...],
  scrollBehavior(to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})

scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。


121、动态路由：添加动态路由router.addRoute(/**路由记录 */)
 添加之后要手动通过push或者replace方法来触发更新。
在路由全局前置守卫中添加router.addRoute 通过return to.fullPath来重定向刷新页面。

删除路由：router.removeRoute(路由名称),或者调用addRoute返回的回调。
url[https://router.vuejs.org/zh/guide/advanced/dynamic-routing.html]


122、在pinia中可以使用路由，可以访问应用层面provide的数据。

123、在pinia中如果要解构一个store, 要调用storeToRefs,不能直接解构。

124、在pinia中state是返回初始状态的函数，例如：
const useStore = defineStore('storeId', {
  state: () => {
    return {
      // 用于初始化空列表
      userList: [] as UserInfo[],
      // 用于尚未加载的数据
      user: null as UserInfo | null,
    }
  },
})
通常可以直接通过store.userList来访问state,但是必须是在state中定义了初始值的，不能访问修改不存在属性的值。
在选项式API中可以通过。store.$reset()重置为初始值，在Setup store需要自己定义重置方法。
变更state 可以通过$patch方法，$patch可以接受一个对象覆盖state同名属性，也可以接受一个函数，函数的第一个参数是当前state，直接访问
state属性来修改。

你可以通过 store 的 $subscribe() 方法侦听 state 及其变化。比起普通的 watch()，使用 $subscribe() 的好处是 subscriptions 在 patch 后只触发一次
同样也可以使用watch侦听


125、Getter 完全等同于 store 的 state 的计算值。可以通过 defineStore() 中的 getters 属性来定义它们。
推荐使用箭头函数，并且它将接收 state 作为第一个参数：
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0,
  }),
  getters: {
    doubleCount: (state) => state.count * 2,
  },
})

126、Action 相当于组件中的 method。它们可以通过 defineStore() 中的 actions 属性来定义，并且它们也是定义业务逻辑的完美选择。
类似 getter，action 也可通过 this 访问整个 store 实例

127、你可以通过 store.$onAction() 来监听 action 和它们的结果。传递给它的回调函数会在 action 本身之前执行。after 表示在 promise 解决之后，
允许你在 action 解决后执行一个回调函数。同样地，onError 允许你在 action 抛出错误或 reject 时执行一个回调函数。这些函数对于追踪运行时错误非常有用
onst unsubscribe = someStore.$onAction(({
    name, // action 名称
    store, // store 实例，类似 `someStore`
    args, // 传递给 action 的参数数组
    after, // 在 action 返回或解决后的钩子
    onError, // action 抛出或拒绝的钩子
})=>{})

默认情况下，action 订阅器会被绑定到添加它们的组件上(如果 store 在组件的 setup() 内)。这意味着，当该组件被卸载时，它们将被自动删除。
如果你想在组件卸载后依旧保留它们，请将 true 作为第二个参数传递给 action 订阅器，以便将其从当前组件中分离：
// 此订阅器即便在组件卸载之后仍会被保留
someStore.$onAction(callback, true)

128、插件是通过 pinia.use() 添加到 pinia 实例的。最简单的例子是通过返回一个对象将一个静态属性添加到所有 store。
import { createPinia } from 'pinia'

// 创建的每个 store 中都会添加一个名为 `secret` 的属性。
// 在安装此插件后，插件可以保存在不同的文件中
function SecretPiniaPlugin() {
  return { secret: 'the cake is a lie' }
}

const pinia = createPinia()
// 将该插件交给 Pinia
pinia.use(SecretPiniaPlugin)

// 在另一个文件中
const store = useStore()
store.secret // 'the cake is a lie'

Pinia 插件是一个函数，可以选择性地返回要添加到 store 的属性。它接收一个可选参数，即 context。

export function myPiniaPlugin(context) {
  context.pinia // 用 `createPinia()` 创建的 pinia。
  context.app // 用 `createApp()` 创建的当前应用(仅 Vue 3)。
  context.store // 该插件想扩展的 store
  context.options // 定义传给 `defineStore()` 的 store 的可选对象。
  // ...
}


129、如果你想给 store 添加新的 state 属性或者在服务端渲染的激活过程中使用的属性，你必须同时在两个地方添加它。。

在 store 上，然后你才可以用 store.myState 访问它。
在 store.$state 上，然后你才可以在 devtools 中使用它，并且，在 SSR 时被正确序列化(serialized)。
url[https://pinia.vuejs.org/zh/core-concepts/plugins.html#adding-new-external-properties]


130、当添加外部属性、第三方库的类实例或非响应式的简单值时，你应该先用 markRaw() 来包装一下它，再将它传给 pinia。下面是一个在每个 store 中添加路由器的例子：
import { markRaw } from 'vue'
// 根据你的路由器的位置来调整
import { router } from './router'

pinia.use(({ store }) => {
  store.router = markRaw(router)
})


131、你也可以在插件中使用 store.$subscribe 和 store.$onAction 。

pinia.use(({ store }) => {
  store.$subscribe(() => {
    // 响应 store 变化
  })
  store.$onAction(() => {
    // 响应 store actions
  })
})


132、在定义 store 时，可以创建新的选项，以便在插件中使用它们。
defineStore('search', {
  actions: {
    searchContacts() {
      // ...
    },
  },

  // 这将在后面被一个插件读取
  debounce: {
    // 让 action searchContacts 防抖 300ms
    searchContacts: 300,
  },
})


133、注意，在使用 setup 语法时，自定义选项作为第 3 个参数传递：
defineStore(
  'search',
  () => {
    // ...
  },
  {
    // 这将在后面被一个插件读取
    debounce: {
      // 让 action searchContacts 防抖 300ms
      searchContacts: 300,
    },
  }
)