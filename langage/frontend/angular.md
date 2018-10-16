# Angular

---

[官方文档](https://www.angular.cn/docs)


## Demo
```html
<body ng-app="myApp" ng-init="initData='initData'" ng-controller="ctrl">
    <input type="text" ng-model="initData" />
    <h1>{{initData}}</h1>
    <ul>
        <li ng-repeat="item in list">{{item.name}}:{{item.id}}</li>
        <li ng-repeat="(x,y) in obj">{{x}}:{{y}}</li>
    </ul>
    <select ng-init="selectedItem=list[0]" ng-model="selectedItem" ng-options="x.name for x in list"></select>
    <input ng-click="clickEvent()" type="button" value="click me" />
    <ul>
        <li><a href="#/" alt="route_view">DefaultView</a></li>
        <li><a href="#/route_view" alt="route_view">RouteView</a></li>
    </ul>
    <div ng-include="include.html"></div>
    <div ng-view></div>
    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
    <!--路由模块-->
    <script src="http://apps.bdimg.com/libs/angular-route/1.3.13/angular-route.js"></script>
    <!--动画模块-->

    <script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular-animate.min.js"></script>
    <script>
        /*Core*/
        var mainApp = angular.module("myApp", ["ngRoute"])
            .controller("ctrl",
                function ($scope) {
                    $scope.list = [
                        { name: "A", id: "1" },
                        { name: "B", id: "2" }
                    ];
                    $scope.obj = {
                        name: "XXX",
                        age: "12",
                        sex: "男"
                    };
                    $scope.clickEvent = function () {
                        console.info("call click event!");
                    }
                });
        /*DI*/
        //向控制器传递参数
        mainApp.value("defaultInput", 5);
        //注册控制器
        mainApp.controller('CalcController', function ($scope, CalcService, defaultInput) {
            $scope.number = defaultInput;
            $scope.result = CalcService.square($scope.number);
            $scope.square = function () {
                $scope.result = CalcService.square($scope.number);
            }
        });
        //注册服务
        mainApp.service('CalcService', function (MathService) {
            this.square = function (a) {
                return MathService.multiply(a, a);
            }
        });
        //创建一个 service、factory等
        mainApp.config(function ($provide) {
            $provide.provider('MathService', function () {
                this.$get = function () {
                    var factory = {};
                    factory.multiply = function (a, b) {
                        return a * b;
                    }
                    return factory;
                };
            });
        });
        //在 service 和 controller 需要时创建
        //mainApp.factory('MathService', function () {
        //    var factory = {};
        //    factory.multiply = function (a, b) {
        //        return a * b;
        //    }
        //    return factory;
        //});
        //在配置阶段传递数值
        //mainApp.constant("configParam", "constant value");
        /*Route*/
        mainApp.config(function ($routeProvider) {
            $routeProvider
                .when('/', { template: '这是首页页面' })
                .when('/route_view', { template: '这是电脑分类页面' })
                .otherwise({ redirectTo: '/' });
        });
        /*Service*/
        mainApp.controller('myCtrl', function ($scope, $location, $timeout) {
            $scope.myHeader = "Hello World!";
            $timeout(function () {
                $scope.myHeader = "How are you today?";
            }, 2000);
            //监听数据变化
            $scope.$watch('lastName', function () {
                $scope.fullName = $scope.lastName + " " + $scope.firstName;
            });

        });
    </script>
</body>
```

## Service
* **$location**：*类似 window.location*
* **$timeout**：*window.setTimeout*
* **$interval**：*window.setInterval *
* **$watch**：*监听数据变化*
* **$$http**：
```txt
$http({
method: 'GET',
url: '/someUrl'
}).then(function successCallback(response) {
// 请求成功执行代码
}, function errorCallback(response) {
// 请求失败执行代码
});
```

## 指令

* `ng-app` 初始化一个应用程序
* `ng-init` 初始化数据 *ng-init="key='value';nbr=3"*
* `ng-controller` 控制器
* `ng-model` 双向绑定
* `ng-bind` 绑定数据  *{{}}*
* `ng-repeat` 重复元素

```javascript
<select><option ng-repeat="item in list">{{item}}</opt
ion></select>
```

* `ng-options` 选择框，作用于select
* `ng-disabled` 绑定至HTML元素的disabled属性，指示是否可用
* `ng-show` 是否显示
* `ng-hide` 是否隐藏

