# Vue

---

[官方文档](https://cn.vuejs.org/v2/guide/)

## Init

```bash
npm --registry https://registry.npm.taobao.org info underscore
npm install -g vue-cli  
vue init webpack <my-project-name> 
npm install
npm run dev
```



## Demo
```html
<body>   
    <style>
        .class1 {
            background-color: #808080;
            color: red;
        }
    </style>
    <div id="app">
        <p>{{message | capitalize}}：{{computedMessage}}</p>
        <label for="r1">修改颜色</label><input type="checkbox" v-model="class1" id="r1" />
        <br />
        <div v-bind:class="{'class1':class1}">
            directiva v-bind:class
        </div>
        <div v-bind:class="[class1]">
            directiva v-bind:class
        </div>
        <button v-on:click="clickHandler">click me, look console</button>
        <!--频繁切换用show 条件变化可能不大用if-->
        <div v-if="class1">v-if</div>
        <div v-show="class1">v-show</div>
        <ul>
            <li v-for="x in list">{{x}}</li>
            <li v-for="obj in objs">{{obj.name}}：{{obj.sex}}</li>
            <li v-for="(key,value,index) in obj">{{index}}、{{key}}：{{value}}</li>
        </ul>
        <ul v-for="n in 4" v-bind:key="n">
            <li>{{n}}</li>
        </ul>
        <runoob></runoob>
        <tag-name v-bind:message="message"></tag-name>
        <p>
            <router-link to="/">Go To Root</router-link>
            <router-link to="/foo">Go To Foo</router-link>
            <router-link to="/bar">Go To Bar</router-link>
        </p>
        <router-view></router-view>
    </div>
    <script src="https://cdn.bootcss.com/vue/2.2.2/vue.min.js"></script>

    <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
    <script>
        //模板
        Vue.component("tag-name", {
            props: {
                message: {
                    required: true,
                    type: String,
                    validator: function (value) {
                        console.info('validator data:' + value);
                        return value !== "";
                    },
                    default: function () {
                        return "default message";
                    }
                }
            },
            template: '<div>{{message}}</div>'
        });
        //自定义指令
        Vue.directive('focus',
            {
                //el:元素 binding:{name:指令名,不包括v-,value:绑定的值,oldValue:绑定的前一个值,expression:绑定值的字符串形式,arg:参数,modifiers:包含修饰符的对象}
                inserted: function (el, binding, vnode, oldVnode) {//当绑定元素插入到 DOM 中
                    el.focus();
                },
                bind: function (el) {//第一次绑定到元素时调用
                    el.focus();
                },
                update: function (el) {//所在模板更新时调用
                    el.focus();
                },
                componentUpdated: function (el) {//完成更新时调用
                    el.focus();
                },
                unbind: function (el) {//解绑时调用、仅一次
                    el.focus();
                }
            });
        var router = new VueRouter({
            routes: [{
                path: '/',
                component: {
                    template: "<div>root</div>"
                }
            }, {
                path: '/foo',
                component: {
                    template: "<div>foo</div>"
                }
            }, {
                path: '/bar',
                component: {
                    template: "<div>bar</div>"
                }
            }]
        });
        var mainApp = new Vue({
            el: "#app",
            data: { //数据
                message: "first Vue Data",
                class1: true,
                list: ["A", "B", "C"],
                objs: [
                    { name: "X", sex: "男" },
                    { name: "Y", sex: "女" },
                    { name: "Z", sex: "男" }
                ],
                obj: {
                    name: "job",
                    address: "TJ",
                    company: "中原"
                }
            },
            methods: { //方法
                clickHandler: function () {
                    console.info("call click event");
                }
            },
            //计算属性（会被缓存）
            computed: {
                computedMessage: {
                    get: function () {
                        return this.message.toUpperCase();
                    },
                    set: function (val) {
                        this.message = val;
                    }
                }
            },
            filters: { //过滤器
                capitalize: function (value) {
                    if (!value) return '';
                    value = value.toString();
                    return value.charAt(0).toUpperCase() + value.slice(1);
                }
            },
            //局部组件
            components: {
                'runoob': {
                    template: "<h1>sdfsdf</h1>"
                }//将只在你模块中可用
            },
            router: router,
            //局部指令
            //directives: {
            //    fucus: {
            //        inserted: function (el) {
            //            el.focus();
            //        }
            //    }
            //}
            //监听某个属性 实现全选
            //watch: {
            //    "checkedNames": function () {
            //        if (this.checkedNames.length == this.checkedArr.length) {
            //            this.checked = true
            //        } else {
            //            this.checked = false
            //        }
            //    }
            //}
        });
        //元素
        //mainApp.$el = document.getElementById('id');
        //数据
        //mainApp.$data = {};
        //绑定当message改变时执行的方法
        //mainApp.$watch('message',
        //    function (oldVal, newVal) {
        //
        //    });
    </script>
</body>
```