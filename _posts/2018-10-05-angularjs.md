---
layout: post
title: AngularJS
date: '2018-10-05 00:27'
---

AngularJS is a JavaScript framework. It can be added to an HTML page with a `<script>` tag.

```xml
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular.min.js"></script>
```

It is recommended that you load the AngularJS library either in the `<head>` or at the start of the `<body>`, so that the `angular.module` function can be loaded..
AngularJS modules define AngularJS applications. AngularJS controllers control AngularJS applications.

This reference is the summary of [W3School's Angular tutorial](https://www.w3schools.com/angular/default.asp).

* TOC
{:toc}

# Directives
AngularJS extends HTML with **ng-directives**. AngularJS directives are HTML attributes with an `ng` prefix.

AngularJS has a set of built-in directives which you can use to add functionality to your application.
* The `ng-app` directive defines an AngularJS application, and defines the "owner" of the AngularJS application in terms of HTML element. It will **auto-bootstrap** the application when a webpage is loaded.
* The `ng-controller` directive defines the controller.
* The `ng-model` directive binds the value of HTML controls (input, select, textarea) to *application data*.
* The `ng-repeat` directive repeats an HTML element: `ng-repeat="x in names"` iterates over an array `names` and clones HTML elements once for each item in a collection.

The **ng-bind** directive binds application data to the HTML view. The `ng-init` directive initializes AngularJS application variables.

You can use `data-ng-`, instead of `ng-`, if you want to make your page HTML valid. Using `ng-init` is not very common.

## Create New Directives
In addition to all the built-in AngularJS directives, you can create your own directives. New directives are created by using the `app.directive` function. To invoke the new directive, make an HTML element with the same tag name as the new directive. When naming a directive, you must use a camel case name, `w3TestDirective`, but when invoking it, you must use `-` separated name, `w3-test-directive`:
```javascript

<body ng-app="myApp">

<w3-test-directive></w3-test-directive>

<script>
var app = angular.module("myApp", []);
app.directive("w3TestDirective", function() {
    return {
        template : "<h1>Made by a directive!</h1>"
    };
});
</script>

</body>
```
You can invoke a directive by using:
* Element name: `<w3-test-directive></w3-test-directive>`
* Attribute: `<div w3-test-directive></div>`
* Class: `<div class="w3-test-directive"></div>`
* Comment: `<!-- directive: w3-test-directive -->`

You can restrict your directives to only be invoked by some of the methods. The legal restrict values are:
* E for Element name
* A for Attribute
* C for Class
* M for Comment

By default the value is `EA`, meaning that both Element names and attribute names can invoke the directive.

## `ng-repeat`
The `ng-repeat` directive is perfect for displaying lists or tables.

{% raw %}
```xml
 <div ng-app="myApp" ng-controller="customersCtrl">

<table>
  <tr ng-repeat="x in names">
    <td>{{ $index + 1 }}</td>
    <td>{{ x.Name }}</td>
    <td>{{ x.Country }}</td>
  </tr>
</table>

</div>

<script>
var app = angular.module('myApp', []);
app.controller('customersCtrl', function($scope, $http) {
    $http.get("customers.php")
    .then(function (response) {$scope.names = response.data.records;});
});
</script>
```
{% endraw %}

`$index` is the table index.

## `ng-model`
With the `ng-model` directive you can bind the value of an input field to a variable created in AngularJS. The binding goes both ways. If the user changes the value inside the input field, the AngularJS property will also change its value.

One usecase of `ng-model` directive is to provide type validation for application data (number, e-mail, required):
```xml
<form ng-app="" name="myForm">
    Email:
    <input type="email" name="myAddress" ng-model="text">
    <span ng-show="myForm.myAddress.$error.email">Not a valid e-mail address</span>
</form>
```
The `ng-model` directive can provide status for application data (invalid, dirty, touched, error).

The `ng-model` directive provides CSS classes for HTML elements, depending on their status:
```xml
 <style>
input.ng-invalid {
    background-color: lightblue;
}
</style>
<body>

<form ng-app="" name="myForm">
    Enter your name:
    <input name="myName" ng-model="myText" required>
</form>
```

The `ng-model` directive adds/removes the following classes, according to the status of the form field:
* ng-empty
* ng-not-empty
* ng-touched
* ng-untouched
* ng-valid
* ng-invalid
* ng-dirty
* ng-pending
* ng-pristine

## Data Binding
The HTML container where the AngularJS application is displayed, is called the view. Data binding in AngularJS is the synchronization between the model and the view. The view has access to the model, and there are several ways of displaying model data in the view.
* The `ng-bind` directive, which will bind the innerHTML of the element to the specified model property: `<p ng-bind="firstname"></p>`
* double braces {% raw %}`{{ }}`{% endraw %}
* `ng-model` directive: `<input ng-model="firstname">`

When data in the model changes, the view reflects the change, and when data in the view changes, the model is updated as well. This happens immediately and automatically, which makes sure that the model and the view is updated at all times.

AngularJS data can be defined in the controllers as well.
```javascript
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.firstname = "John";
    $scope.lastname = "Doe";
});
```

## `ng-options`
If you want to create a dropdown list, based on an object or an array in AngularJS, you should use the `ng-options` directive. Though you can use `ng-repeat` with `<select>`, `ng-options` returns the select object instead of only string:

{% raw %}
```xml

<select ng-model="selectedCar" ng-options="x.model for x in cars">
</select>

<h1>You selected: {{selectedCar.model}}</h1>
<p>Its color is: {{selectedCar.color}}</p>

```
{% endraw %}

If the data source is an object with key-value pairs, use `ng-options="k for (k, v) in cars"`. `k` is the key and `v` is the value. The selected value will always be the value in the key-value pair.

## Directive for DOM Operations
The `ng-disabled` directive binds AngularJS application data to the **disabled** attribute of HTML elements.

The `ng-show` directive shows or hides an HTML element based on the value of `ng-show`. The `ng-hide` directive hides or shows an HTML element.

# Expressions

AngularJS expressions are written inside double braces: `{{ expression }}`, or inside a directive: `ng-bind="expression"`. AngularJS will "output" data exactly where the expression is written. AngularJS expressions are much like JavaScript expressions: They can contain literals, operators, and variables.

Example:

{% raw %}
```xml
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
<p>The name is {{ firstName + " " + lastName }}</p>
</div>
```
{% endraw %}

```xml
<div ng-app="" ng-init="firstName='John';lastName='Doe'">
<p>The name is <span ng-bind="firstName + ' ' + lastName"></span></p>
</div>
```

AngularJS expressions are only evaludated when the `ng-app` directive presents.

Numbers, strings, objects, arrays in AngularJS is like in JavaScript. AngularJS expressions do not support conditionals, loops, and exceptions, while JavaScript expressions do. AngularJS expressions support filters, while JavaScript expressions do not.

# Modules
An AngularJS module defines an application. The module is a container for the different parts of an application. The module is a container for the application controllers. Controllers always belong to a module.

A module is created by using the AngularJS function `angular.module`. A controler can be added to a module, and is referred in HTML with the `ng-controller` directive.

{% raw %}
```javascript
<div ng-app="myApp" ng-controller="myCtrl">{{ firstName + " " + lastName }}</div>

<script>

var app = angular.module("myApp", []);

app.controller("myCtrl", function($scope) {
* $scope.firstName = "John";
* $scope.lastName = "Doe";
});

</script>
```
{% endraw %}

It is common in AngularJS applications to put the module and the controllers in JavaScript files. They are linked to the HTML files via `<script>` tag.

The reason that AngularJS ties everything to the module is to NOT have global functions as they can easily be overriden or destroyed by other scripts.

# Controller
AngularJS controllers **control the data** of AngularJS applications. They are regular `JavaScript Objects`. The `ng-controller` directive defines the application controller.

Example:

{% raw %}
```javascript
<div ng-app="myApp" ng-controller="myCtrl">

First Name: <input type="text" ng-model="firstName"><br>
Last Name: <input type="text" ng-model="lastName"><br>
<br>
Full Name: {{firstName + " " + lastName}}

</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
* $scope.firstName = "John";
* $scope.lastName = "Doe";
});
</script>
```
{% endraw %}

Explanation:
* The AngularJS application is defined by `ng-app="myApp"`. The application runs inside the `<div>`.
* The `ng-controller="myCtrl"` attribute is an AngularJS directive. It defines a controller.
* The `myCtrl` function is a JavaScript function.
* AngularJS will invoke the controller with a `$scope` object. `$scope` is the application object (the owner of application variables and functions).
* The controller creates two properties (variables) in the scope (`firstName` and `lastName`).
* The `ng-model` directives bind the input fields to the controller properties (`firstName` and `lastName`).

The controller can define methods on the `$scope` object via variables as functions:
```javascript
$scope.fullName = function() {
* return $scope.firstName + " " + $scope.lastName;
};
```

## Scope
The scope is the binding part between the HTML (view) and the JavaScript (controller). The scope is an object with the available properties and methods. The scope is available for both the view and the controller.

When adding properties to the `$scope` object in the controller, the view (HTML) gets access to these properties. In the view, you do not use the prefix `$scope`, you just refer to a propertyname.

If we consider an AngularJS application to consist of:
* View, which is the HTML.
* Model, which is the data available for the current view.
* Controller, which is the JavaScript function that makes/changes/removes/controls the data.

Then the scope is the Model.

It is important to know which scope you are dealing with, at any time. But for larger applications there can be sections in the HTML DOM which can only access certain scopes. For example, when dealing with the `ng-repeat` directive, each repetition has access to the current repetition object.

All applications have a `$rootScope` which is the scope created on the HTML element that contains the ng-app directive. The `$rootScope` is available in the entire application. If a variable has the same name in both the current scope and in the rootScope, the application use the one in the current scope.

# Filters
Filters can be added in AngularJS to format data.

* `currency` Format a number to a currency format.
* `date` Format a date to a specified format.
* `filter` Select a subset of items from an array. It only works on arrays and returns an array.
* `json` Format an object to a JSON string.
* `limitTo` Limits an array/string, into a specified number of elements/characters.
* `lowercase` Format a string to lower case.
* `number` Format a number to a string.
* `orderBy` Orders an array by an expression.
* `uppercase` Format a string to upper case.

Filters can be added to expressions by using the pipe character `|`, followed by a filter.

{% raw %}
```xml
<div ng-app="myApp" ng-controller="personCtrl">
<p>The name is {{ lastName | uppercase }}</p>
</div>
```
{% endraw %}

Similarly, filters can be added to directives, such as `ng-repeat`:

{% raw %}
```xml
<div ng-app="myApp" ng-controller="namesCtrl">

<ul>
  <li ng-repeat="x in names | orderBy:'country'">
    {{ x.name + ', ' + x.country }}
  </li>
</ul>

</div>
```
{% endraw %}

You can make your own filters by registering a new filter factory function with your module using `app.filter`:

{% raw %}
```javascript

<ul ng-app="myApp" ng-controller="namesCtrl">
    <li ng-repeat="x in names">
        {{x | myFormat}}
    </li>
</ul>

<script>
var app = angular.module('myApp', []);
app.filter('myFormat', function() {
    return function(x) {
        var i, c, txt = "";
        for (i = 0; i < x.length; i++) {
            c = x[i];
            if (i % 2 == 0) {
                c = c.toUpperCase();
            }
            txt += c;
        }
        return txt;
    };
});
app.controller('namesCtrl', function($scope) {
    $scope.names = ['Jani', 'Carl', 'Margareth', 'Hege', 'Joe', 'Gustav', 'Birgit', 'Mary', 'Kai'];
});
</script>
```
{% endraw %}

This example format every other characters to uppercase.

# Services
In AngularJS, a service is a function, or object, that is available for, and limited to, your AngularJS application. AngularJS has about 30 built-in services.

An example is the `$location` service. The `$location` service has methods which return information about the location of the current web page:
```javascript
var app = angular.module('myApp', []);
app.controller('customersCtrl', function($scope, $location) {
    $scope.myUrl = $location.absUrl();
});
```
Note that the `$location` service is passed in to the controller as an argument. In order to use the service in the controller, it must be defined as a dependency.

## The `$http` Service
The `$http` service is one of the most common used services in AngularJS applications. The service makes a request to the server, and lets your application handle the response:

```javascript
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
    $http.get("welcome.htm").then(function (response) {
        $scope.myWelcome = response.data;
    });
});
```
The example above uses the `.get` method of the `$http` service.

The `.get` method is a shortcut method of the $http service. There are several shortcut methods:
* `.delete()`
* `.get()`
* `.head()`
* `.jsonp()`
* `.patch()`
* `.post()`
* `.put()`

The complete way for these shortcut methods are:
```javascript
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
    $http({
        method : "GET",
        url : "welcome.htm"
    }).then(function mySucces(response) {
        $scope.myWelcome = response.data;
    }, function myError(response) {
        $scope.myWelcome = response.statusText;
    });
});
```

The response from the server is an object with these properties:
* `.config` the object used to generate the request.
* `.data` a string, or an object, carrying the response from the server. It is expected to be in JSON format.
* `.headers` a function to use to get header information.
* `.status` a number defining the HTTP status.
* `.statusText` a string defining the HTTP status.

To handle errors, add one more function to `.then` function.

## The `$timeout` Service
The `$timeout` service is AngularJS' version of the `window.setTimeout` function.

```javascript
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $timeout) {
    $scope.myHeader = "Hello World!";
    $timeout(function () {
        $scope.myHeader = "How are you today?";
    }, 2000);
});
```

## The `$interval` Service

The `$interval` service is AngularJS' version of the `window.setInterval` function. To display time every second:

```javascript
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $interval) {
    $scope.theTime = new Date().toLocaleTimeString();
    $interval(function () {
        $scope.theTime = new Date().toLocaleTimeString();
    }, 1000);
});
```

## Create Your Own Service
1. Use `app.service` to define the service and connect it to the application;
2. Declare the service as a depeneency in any controller, directive, filter, or even inside other services.

```javascript
app.service('hexafy', function() {
    this.myFunc = function (x) {
        return x.toString(16);
    }
});
app.controller('myCtrl', function($scope, hexafy) {
    $scope.hex = hexafy.myFunc(255);
});
```

# Routing
The `ngRoute` module helps your application to become a Single Page Application (allows navigation to different pages without page reloading).

To make your applications ready for routing, you must:
1. include the AngularJS Route module:
```xml
<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.8/angular-route.js"></script>
```
2. add the `ngRoute` as a dependency in the application module:
```javascript
var app = angular.module("myApp", ["ngRoute"]);
```

Now your application has access to the route module, which provides the `$routeProvider`. Use the `$routeProvider.when` to configure different routes in your application, or `$routeProvider.otherwise` to define fallback page:
```javascript
var app = angular.module("myApp", ["ngRoute"]);
app.config(function($routeProvider) {
    $routeProvider
    .when("/", {
        templateUrl : "main.htm"
    })
    .when("/london", {
        templateUrl : "london.htm",
        controller : "londonCtrl"
    })
    .when("/paris", {
        templateUrl : "paris.htm",
        controller : "parisCtrl"
    })
    .otherwise({
        template : "<h1>None</h1><p>Nothing has been selected</p>"
    });
});
app.controller("londonCtrl", function ($scope) {
    $scope.msg = "I love London";
});
app.controller("parisCtrl", function ($scope) {
    $scope.msg = "I love Paris";
});
```
With the `$routeProvider` you can also define a controller for each "view". In  the `$routeProvider.when` method, use `templateUrl` property to define the file, or `template` to write HTML directly.

Your application needs a container to put the content provided by the routing. This container is the `ng-view` directive. Any way to define the `ng-view` directive can be used. Applications can only have one `ng-view` directive, and this will be the placeholder for all views provided by the route.
