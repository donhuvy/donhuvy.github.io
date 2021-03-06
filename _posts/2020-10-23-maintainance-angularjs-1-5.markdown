---
layout: post
title:  "Maintainence AngularJS 1.5"
date:   2020-10-23 13:51:00 +0700
categories: JavaScript AngularJS
---
Maintainence AngularJS 1.5

#### Example 1.

```html
<!DOCTYPE html>

<html ng-app>
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script>
  </head>

  <body>
    <div>
      <label>Name:</label>
      <input type="text" ng-model="yourName" placeholder="Enter a name here">
      <hr>
      <h1>Hello {{yourName}}!</h1>
    </div>
  </body>
</html>
```

#### Example 2.

File `todo.js`

```javascript
angular.module('todoApp', [])
    .controller('TodoListController', function () {
        var todoList = this;
        todoList.todos = [
            { text: 'learn AngularJS', done: true },
            { text: 'build an AngularJS app', done: false }];

        todoList.addTodo = function () {
            todoList.todos.push({ text: todoList.todoText, done: false });
            todoList.todoText = '';
        };

        todoList.remaining = function () {
            var count = 0;
            angular.forEach(todoList.todos, function (todo) {
                count += todo.done ? 0 : 1;
            });
            return count;
        };

        todoList.archive = function () {
            var oldTodos = todoList.todos;
            todoList.todos = [];
            angular.forEach(oldTodos, function (todo) {
                if (!todo.done) todoList.todos.push(todo);
            });
        };
    });
```

File `todo.css`

```css
.done-true {
  text-decoration: line-through;
  color: grey;
}
```

File `index.html`

```html
<!DOCTYPE html>
<html ng-app="todoApp">

<head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.0/angular.min.js"></script>
    <script src="todo.js"></script>
    <link rel="stylesheet" href="todo.css">
</head>

<body>
    <h2>Todo</h2>
    <div ng-controller="TodoListController as todoList">
        <span>{{todoList.remaining()}} of {{todoList.todos.length}} remaining</span>
        [ <a href="" ng-click="todoList.archive()">archive</a> ]
        <ul class="unstyled">
            <li ng-repeat="todo in todoList.todos">
                <label class="checkbox">
                    <input type="checkbox" ng-model="todo.done">
                    <span class="done-{{todo.done}}">{{todo.text}}</span>
                </label>
            </li>
        </ul>
        <form ng-submit="todoList.addTodo()">
            <input type="text" ng-model="todoList.todoText" size="30" placeholder="add new todo here">
            <input class="btn-primary" type="submit" value="Add">
        </form>
    </div>
</body>

</html>
```

### Example 2: `ng-app` in specific 

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8">
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>
</head>
<body>
Input name: <input type="text" ng-model="name"/><br/>
Display: <div>{{name}}</div>

<div>
    10 + 20 = {{10 + 20}}
</div>

<div ng-app>
    10 + 20 = {{10 + 20}}
</div>

</body>
</html>
```

### Example 3: `ng-app` in HTML element

```html
<!DOCTYPE html>
<html ng-app>
<head>
    <title></title>
    <meta charset="utf-8">
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>
</head>
<body>
Input name: <input type="text" ng-model="name"/><br/>
Display: <div>{{name}}</div>

<div>
    10 + 20 = {{10 + 20}}
</div>

<div>
    10 + 20 = {{10 + 20}}
</div>

</body>
</html>
```

![result](/images/2020_10_23_angularjs_01.png)

### Example 4: Module and controller

File `intro.html`

```html
<!DOCTYPE html>
<html ng-app="myModule">
<head>
    <title></title>
    <meta charset="utf-8">
    <!-- <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>-->
    <script src="scripts/angular.min.js"></script>
    <script src="scripts/app.js"></script>
</head>
<body>
<div ng-controller="myController">
    {{message}}
</div>
</body>
</html>
```

File `app.js`

```javascript
var myApp = angular.module('myModule', []);

myApp.controller('myController', myController);

function myController($scope) {
    $scope.message = "Angular JS Application";
}
```

### Example 5: `ng-app` and `ng-controller`

File `app.js`

```javascript
var myApp = angular.module('myModule', []);

myApp.controller('myController', myController);

// create a module.
function myController($scope) {
    $scope.message = "Angular JS Application";
}
```

```html
<!DOCTYPE html>
<html ng-app="myModule">
<head>
    <title></title>
    <meta charset="utf-8">
    <!-- <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>-->
    <script src="scripts/angular.min.js"></script>
    <script src="scripts/app.js"></script>
</head>
<body ng-controller="myController">
<div>
    {{message}}
</div>
<div>
    {{message}}
</div>
</body>
</html>
```

### Example 6: Binding

File `intro.html`

```html
<!DOCTYPE html>
<html ng-app="myModule">
<head>
    <title></title>
    <meta charset="utf-8">
    <!-- <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>-->
    <script src="scripts/angular.min.js"></script>
    <script src="scripts/app.js"></script>
</head>
<body ng-controller="myController">
<div>
    First Name: <input type="text" value="{{employee.FirstName}}"><br/>
    Last Name: {{employee.LastName}}<br/>
    Gender: {{employee.Gender}}<br/>
</div>
</body>
</html>
```

File `app.js`

```js
var myApp = angular.module('myModule', []);

myApp.controller('myController', function ($scope){
    var employee = {
        FirstName: "Mark",
        LastName: "Hastings",
        Gender: "Male"
    }
    $scope.employee = employee;
});
```

other syntax

```html
<div>
    First Name: <input type="text" value="{{employee.FirstName}}"><br/>
    Last Name: {{employee.LastName}}<br/>
    Gender: {{employee.Gender}}<br/>
</div>
```

![result](/images/2020_10_23_angularjs_02.png)

other syntax

```js
var myApp = angular.module('myModule', []).controller('myController', function ($scope) {
    var employee = {
        FirstName: "Vy",
        LastName: "Donhu",
        Gender: "Male"
    }
    $scope.employee = employee;
});
```

### Example 7: Scope

File `app.js`

```js
var myApp = angular.module('scopeDemo', []);
myApp.controller('classController', classController);
myApp.controller('schoolController', schoolController);
myApp.controller('topController', topController);

function classController($scope) {

};

function schoolController($scope) {
    $scope.name = "Child Controller";
}

function topController($scope, $rootScope) {
    $scope.name = "This is nested scope AngularJS";
}
```

File `intro.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8">
    <!-- <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.5.5/angular.min.js"></script>-->
    <script src="scripts/angular.min.js"></script>
    <script src="scripts/app.js"></script>
</head>
<body ng-app="scopeDemo">
<div ng-controller="topController">
    <div ng-controller="classController">
        <div>{{name}}</div>
    </div>
    <div ng-controller="schoolController">
        <div>{{name}}</div>
    </div>
    {{name}}
</div>
</body>
</html>
```

![result](/images/2020_10_23_angularjs_03.png)