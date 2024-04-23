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




