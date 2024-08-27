1、源码中引入react.js 和 react.dom.js

2、ReactDom.render(React.DOM.h1(null,"hello,world"),document.getElementById('app')) 创建应用

3、创建组件let Component = React.createClass({
    render:function(){
        return React.DOM.span(null,"I'm so custom!")
    }
})

ReactDOM.render(
    React.createElement(Component);// 可以通过传入第二个参数来向组件内传值，在组件内的render方法里使用this.props.xxx获取。
    document.getElementById("app")
)

4、在定义组件的React.createClass方法里可以传一个propTypes的属性，用以定义传入的参数类型。

React.createClass({
    propTypes:{
        name:React.PropTypes.string.isRequired //必须提供的字段，如果没有收到这个值，浏览器将会报警告信息。
        age:React.PropTypes.number
    },
    render:()=>{
        return React.DOM.span(null,"My name  is " + this.props.name)
    },
    getDefaultProps:function(){     //使用getDefaultProps 可以给组件不是必传的参数指定默认值。
        return {
            age:0
        }
    }
})

5、让组件产生响应式数据在4的基础上，第一步增加getInitialState（初始化state）属性方法，返回一个对象,这个对象的属性值，可以通过this.state.xxx读出，可以使用this.setState(param:{xxx:值}) 改变这个state的值。setState方法会触发render方法，重新渲染组件 与replaceState不同，setState会把属性与当前的this.state合并。replaceState方法会替换掉当前的state。


6、React对事件实例进行了处理，用以兼容各种浏览器之间的差别。

7、React 的生命周期方法：
    componentWillMount：在渲染前调用
    componentDidMount：在第一次渲染后调用
    componentWillUpdate(object nextProps, object nextState)：在接收到新的props或者state前调用
    componentDidUpdate(object prevProps, object prevState)：在接收到新的props或者state后调用
    componentWillUnmount()：在删除组件之前立刻调用
    shouldComponentUpdate(object nextProps, object nextState)在componentWillUpdate之前调用，返回一个布尔值。在组件接收到新的props或者state时被调用。
    在初始化时或者使用forceUpdate方法时不被调用。

8、mixins:
    mixins是react组件中一个比较特殊的概念。简单的说，react中 mixins 就是一个包含各种组件逻辑的 JavaScript 对象，这些逻辑包括组件的初始化、生命周期的操作、组件的state等等。
    mixins的引入，是为了让组件可以复用一些可复用的逻辑。




React 16学习：

1、react 声明函数组件 

    function Welcome(props) {   // 函数名必须大写，否则会当成一个普通的react元素（type为字符串），而不是组件元素(type 为函数)
      return <h1>Hello, {props.name}</h1>;
    }

    使用：

    ReactDom.render(<Welcome  />,document.getElementById('app'))

2、react 声明类组件 

    class Welcome extends React.Component {
      render() {    // 必须要提供render方法
        return <h1>Hello, {this.props.name}</h1>;
      }
    }  
    
    使用：

    ReactDom.render(<Welcome  />,document.getElementById('app')) 


3、函数组件传参：
    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }
    使用： 
    <Welcome name="Sara" /> 组件属性要使用小驼峰命名法
    组件传递的参数是只读的，不能修改（数据属于谁，谁才能修改）

4、类组件传参：
    class Welcome extends React.Component {

      constructor(props) {
        super(props); // 调用父类的构造函数，相当于this.props = props;
      }
      /**构造函数也可省略 */
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }
    使用：
    <Welcome name="Sara" />  组件属性要使用小驼峰命名法
    组件传递的参数是只读的，不能修改（数据属于谁，谁才能修改）


5、this.setState方法可以触发组件更新。


6、组件的绑定事件要使用小驼峰命名法。
const btn = <button onClick={handleClick}>Click me</button>
function handleClick() {
  console.log('Clicked!');
}

ReactDom.render(
  btn,
  document.getElementById('app')
)


7、在React中，组件的事件，本质上就是一个属性
如果没有特殊处理，在事件处理函数中，this指向的是undefined，所以需要手动绑定this指向。
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 这个绑定是必要的，使`this`在回调中起作用
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
      )
  }

绑定事件处理函数的this指向，有三种方式：
1、在构造函数中绑定this指向
2、使用箭头函数绑定this指向
3、在render方法中绑定this指向（不推荐）

为什么事件处理函数的this是undefined？是因为：
1、把事件处理函数，赋值给点击事件，类似于把类中的方法，复制给变量
2、这样一来, 原来的方法是由对象调用，this指向对象，而现在赋值给变量是直接调用，直接调用的话，this会指向window
3、但是，类中方法是局部作用域，都是开启了严格模式，因此this指向是undefined


8、在React中 ,setState是异步的。
9、this.setState()可以接受第二个参数，这个参数是一个回调函数，当state更新完成之后，会自动调用这个回调函数。

10、setState的第一个参数可以是一个函数，如果遇到某个事件中，需要同步调用多次，需要使用函数的方式得到最新状态

11、setState的最佳实践：
     1、把所有的setState当作是异步的
     2、永远不要信任setState调用之后的状态
     3、如果要使用改变之后的状态，需要使用回调函数（setState的第二个参数,回调会正在全部状态更新完成和渲染之后执行）
     4、如果新的状态要根据之前的状态进行运算，使用函数的方式改变状态（setState的第一个参数）


React会对异步的setState进行合并，render方法只会调用一次，所以，如果连续调用多次setState，只会触发一次render方法。


12、函数组件是没有生命周期的，类组件才有生命周期。
    1、组件初始化，调用constructor方法。同一个组件只会调用一次。（不能再挂载到页面之前调用setState）
    2、组件即将挂载到页面componentWillMount,和构造函数一样只会运行一次。（可以使用setState，但可能导致bug）
    3、调用render方法，返回一个虚拟Dom ， 会被挂载到虚拟DOM树中，最终渲染到页面的真实DOM中。可能不止一次运行，只要需要重新渲染，就会重新运行。（严禁使用setState，导致无限递归渲染）

    4、组件挂载到页面componentDidMount，虚拟DOM已经挂载到页面成为真实DOM,只会运行一次。（可以使用setState, 通常会将）
    以上是组件的挂载阶段。

    更新阶段：
    5、componentWillReceiveProps ，组件接收到新的props时调用，第一次渲染不会调用，更新时调用。
    6、shouldComponentUpdate，指示React是否重新渲染该组件，通过返回true 或false ,判断是否执行render方法，返回true，执行render方法，返回false，不执行render方法。
    7、componentWillUpdate，组件即将被重新渲染，更新时调用

    8、render方法，渲染组件
    9、componentDidUpdate，组件已经完成重新渲染，往往在函数中使用DOM操作，改变元素。
    以上是组件的更新阶段。

    卸载阶段：
    10、componentWillUnmount，组件即将卸载，卸载时调用，通常在该函数中销毁一些组件的资源，比如定时器，事件监听器等。
    以上是组件的卸载阶段。
    
新版生命周期：
    初始化阶段：初始化属性和状态调用constructor方法。
    挂载阶段：
    1、static getDerivedStateFromProps(props, state) 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。
    2、render方法，渲染组件
    3、componentDidMount，组件已经完成重新渲染，往往在函数中使用DOM操作，改变元素。

    更新阶段：
    1、static getDerivedStateFromProps(props, state) 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。
    2、shouldComponentUpdate(nextProps, nextState) ，指示React是否重新渲染该组件，通过返回true 或false ,判断是否执行render方法，返回true，执行render方法，返回false，不执行render方法。
    3、render方法，渲染组件
    4、getSnapshotBeforeUpdate(prevProps, prevState)，在最近一次渲染输出（提交到 DOM 节点）之前调用。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 componentDidUpdate()的第三个参数。
    5、componentDidUpdate(prevProps, prevState, snapshot)，组件已经完成重新渲染，往往在函数中使用DOM操作，改变元素。

    卸载阶段：
    6、componentWillUnmount，组件即将卸载，卸载时调用，通常在该函数中销毁一些组件的资源，比如定时器，事件监听器等。


父组件可以在子组件调用的标签中间传入要插入子组件的内容，类似于插槽的功能。子组件通过this.props.children获取到父组件传入的内容，然后渲染到子组件中。
当然也可通过属性直接传递要渲染的内容


受控组件和非受控组件：
受控组件：组件的使用者，有能力完全控制该组件的行为和内容。通常情况下，受控组件往往没有自身的状态，其内容完全受到属性的控制。
非受控组件：组件的使用者，没有能力控制该组件的行为和内容，组件的行为和内容完全自行控制。


表单组件，默认情况下是非受控组件，一旦设置了表单组件的value属性变为受控组件

input 文本输入框  绑定value属性，通过onChange事件改变value的值
textarea 文本域 绑定value属性，通过onChange事件改变value的值
select 下拉列表 绑定value属性，通过onChange事件改变value的值
checkbox 复选框 绑定checked属性，通过onChange事件改变checked的值
radio 单选框 绑定checked属性，通过onChange事件改变checked的值


属性默认值：通过defaultProps（函数的默认属性）设置组件的默认属性
属性类型检查：通过propTypes（静态属性）设置组件的属性类型，如果类型不匹配，控制台会报错

先混合再进行类型检查


高阶组件：高阶组件就是一个函数，接收一个组件作为参数，返回一个新的组件。高阶组件的作用是用于代码复用，逻辑抽象，便于测试。例如：react-redux中的connect函数就是一个高阶组件。

不要在render中使用高阶组件，因为每次render函数调用时，高阶组件都会返回一个新的组件，导致子组件每次都会重新挂载，影响性能。
不要在高阶组件中修改传入的组件。

ref:获取组件的DOM元素，通过this.refs.refName获取DOM元素。refName可以是一个字符串，也可以是一个函数，如果是函数，函数的参数是DOM元素。

ref如果作用于内置的HTML组件得到的将是真实的DOM元素，如果作用于类组件，得到的将是类的实例。

ref不能作用于函数组件。因为函数组件没有实例。

ref推进赋值对象和函数，对象通过React.createRef()创建，赋值为函数，第一个参数就是dom对象或者类实例对象。componentDidMount的时候会调用该函数，在componentDidMount事件中可以使用ref,如果ref的值发生了变动（旧的函数被新的函数替代），分别调用旧的函数以及新的函数，时间点出现在componentDidUpdate之前。
旧的函数被调用时，传递null,新的函数被调用时，传递dom对象或者类实例对象。

如果ref所在的组件被卸载，会调用函数



ref转发：
React.forwardRef()函数接收一个组件，返回一个新的组件（可以看作高阶组件），新的组件可以传递ref给内部的组件，例如：
const MyComponent = React.forwardRef((props, ref) => (
  <input {...props} ref={ref} />
));

function ParentComponent() {
  const inputRef = React.createRef();
  return <MyComponent ref={inputRef} />;
}



上下文Context：上下文提供了一种在组件树中传递数据的方法，无需在每个层级手动传递props。
使用步骤，上下文只能改组件和他的子组件使用

旧版：
1、给类组件书写静态属性childContextTypes,使用该属性对上下文中的数据类型进行约束
2、添加实例方法getChildContext，该方法返回的对象，即为上下文中的数据，该数据必须满足类型约束，该方法会在每次render之后运行。
  获取上下文数据：
  要求：如果要使用上下文中的数据，组件必须有一个静态属性contextTypes，该属性描述了需要获取上下文中的数据类型
      1、可以在组件的构造函数中，通过第二个参数，获取上下文数据
      2、从组件的context属性中获取上下文数据
      3、在函数组件中，通过第二个参数，获取上下文数据

  上下文中的数据变化：
    1、上下文中依赖组件属性或者状态，则组件或者状态发生变化
    2、子组件改上下文中的数据，通过上下文给定一个方法来改上下文中的数据

    如果祖先组件和父组件都声明了上下文，取值先由父组件到祖先组件的顺序取值


  新版上下文Context：
  创建上下文：通过React.createContext()创建上下文，该方法返回一个对象，该对象有两个属性，一个是Provider，一个是Consumer。
  Provider：用于提供上下文数据的组件，该组件有两个属性，一个是value，一个value属性用于提供上下文数据。

  使用上下文中的数据：
   要求，组件必须有一个静态属性contextType，该属性描述了需要获取上下文中的数据类型，值为React.createContext()返回的对象（这个对象应该单独提一个模块，保正模块间能共享），然后可以通过this.context获取上下文中的数据

    Consumer：用于在函数组件中获取上下文数据的组件，该组件有一个属性value，该属性是一个函数，函数的参数就是上下文数据，函数的返回值就是子组件。也可以直接在标签中间通过一个函数渲染出来

    注意：如果上下文提供者(Context.Provider)中的value属性发生变化，会导致该上下文提供的所有后代元素全部重新渲染，无论该子元素是否优化(shouldComponentUpdate返回什么结果，甚至都不会调用)。

  

