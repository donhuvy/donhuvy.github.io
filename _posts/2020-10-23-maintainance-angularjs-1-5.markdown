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

Example 2.

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

Example 3

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

