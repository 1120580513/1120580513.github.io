# React

---

[官方文档](https://doc.react-china.org/docs/hello-world.html)

## Demo
```html
<script src="https://cdn.bootcss.com/react/15.4.2/react.min.js"></script>
<script src="https://cdn.bootcss.com/react/15.4.2/react-dom.min.js"></script>
<script src="https://cdn.bootcss.com/babel-standalone/6.22.1/babel.min.js"></script>
<div id="example"></div>
<div id="ele"></div>
<div id="component"></div>
<script type="text/babel">
    {/*注释：样式*/}
    var myStyle = {
    fontSize:100,
    color:'#FF0000'
    };
    {/*小写开头的变量渲染HTML*/}
    var myDivElement =
    <div className="for" />;
    {/*大写开头的变量渲染React组件*/}
    var MyComponent = React.createClass({
    render:function(){
    var name = this.state.name;
    return (
    <h1 onClick={this.handlerClick}>{name}</h1>)
    },
    getDefaultProps:function(){
    return {name:"Runoob"}
    },
    getInitialState:function(){
    return {name:"点我增加a"};
    },
    handlerClick:function(){
    this.setState({name:this.state.name+"a"})
    },
    propTypes: {
    name: React.PropTypes.string.isRequired,
    },
    });
    var myElement =
    <MyComponent name='Runoot' />;
    var array = [
    <h1 style={myStyle}>标题{ 3 == true ? 'true' : 'false'}</h1>,
    <h2>标题Two</h2>
    ];
    ReactDOM.render(
    /*注释*/
    <div>{array}{/*注释*/}</div>,
    document.getElementById('example'));
    ReactDOM.render(myDivElement,
    document.getElementById('ele'));
    ReactDOM.render(myElement,
    document.getElementById('component'));
</script>
```