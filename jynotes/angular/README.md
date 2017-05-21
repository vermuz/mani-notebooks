# angularjstudy

My personal repo of notes on AngularJS.

# AngularJS 
- Model view whatever
- Model <-> View
- Model (app) contains a controller which is used by html which becomes a view once you use ng-app (app name)
and within a view i.e. div (you use ng-controller which comes from the ng-app used at the top)
- All AngularJS services start with a $, i.e $scope

# Code Module Whatever

```
var myApp = angular.module('myApplication', []);
myApp.controller('mainController', function() {
});
```

# Dependency Injection
- Giving a function an object
- Rather can creating an object inside a function, you pass it to the function
This is bad,

```
function logPerson() {
	// Note: logPerson is dependent on variable john
	// Something that changes for john has to be done here, making
	// things complicated
	// Create a new person
	var john = new Person('John', 'Doe');
	console.log(john);
}
```
but this is good !
```
function logPerson(person) {
	console.log(person);
}
var john = new Person('Mani', 'Ali');
logPerson(john);
```
```
// If a function contains a certain name that angular knows about, you can inject using
// that
var searchPeople = function searchPeople($scope, lastName, height, age, occupation) {
	return 'Mani Ali';
}
```
# Scope Service
- It is a big part of the thing that binds model to the view.
- It is an object from scope service.
- It involves dependency injection.
- Scope will show a lot of functions, that is AngularJS doing dependency injection
- Inclusion of $scope in function parameters, leads to AngularJS recognizing that and injecting into the 
function

# Using other angular services (Examples)
- Just add it to the dependency list, like
```
var myApp = angular.module('myApp', ['ngMessages', 'ngResource']);

# Usage
myApp.controller('mainController', function($scope, $log, $filter, $resource)
```
- You must include supporting files from code.angularjs for your stuff to work
- Angular-Messages (will give you something like form error handling pretty quick)
- Angular-Resource (good at loading resources pretty quick)


# AngularJS Notes 

# Functions inside arrays
```
var example = [1, '2', function() { alert('Hello'); }];
```

# Global Namespace
We can save our global namespace from pollution
```
var person = 'Mani';
function logPerson() {
    console.log(person);
}

vs

```

```
var maniApp = {};
stevesApp.person = 'Mani';

maniApp.logPerson = function() {
	console.log(maniApp.person);
}
```
# Functions and Strings

```
var searchPeople = function searchPeople(firstName, lastName, height, age, occupation) {
	return 'Mani Ali';
}
```
```
// We can also take a javascript function and convert it into a string
var searchPeopleString = searchPeople.toString();
console.log(searchPeopleString);
```

# Minification
Shrinking size of falls for faster downloads
file.js -> file.min.js

```
myApp.controller('mainController', ['$scope', '$log', function($scope, $log) {
   $log.info($scope);
}
Note: Order does not matter
```

vs minified version of controller, but order matters now

This is also the second way of doing dependency injection in Angular

```
myApp.controller('mainController', ["$scope", "$log", function(a,b)
{b.info(a)}]);
```

# Interpolation
Creating a string by combining strings and placeholders

In HTML,
```
Hello {{ name }}!
```

# Directive
Instruction to AngularJS to manipulate a piece of the DOM
e.g. add a class, hide this or create this. 
In Javascript and JQuery we may have achieved, DOM manipulation
in memory but Angular prefers to use directives.
View can change the model (input textbox -> two way binding -> update model -> update the view)

## Model

So in our model, we had,

```
$scope.handle
// This function defined our model
$scope.lowercasehandle = function() {...} 

# Usage
myApp.controller('mainController', ['$scope', '$filter', function($scope,
$filter) {
    $scope.handle = '';
    $scope.lowercasehandle = function() {
        return $filter('lowercase')($scope.handle);
    };
}]);
```

```
      <MODEL>
        |
        |
   <Angular>
<provides the bits inbetween>
        |
        |
      <VIEW>
```
## View

Using ng-model directive, we can have two way data binding.
In our view, 

```
<input type="text" ng-model="handle" />
<h1>twitter.com/{{ lowercasehandle() }}</h1>
```

## The Event Loop

On key press, while we are listening for an event,
we respond to it when it happens - it will do that everytime an event it is 
listening for is triggered

```
var tb = document.getElementByIdI("name");
tb.addEventListener("keypress",
     function(event) {
         console.log("Pressed");
     });
```
## Watchers and the Digest Loop

The specific way Angular carries out binding of the model to the view.
The Javascript event loop is listening for keypress, click, mouseover, change etcetera
AngularJS is actually adding event listeners for you and it is extending event loop,
doing more with it, to control binding between model and the event loop

## AngularJS Context and event loop

The Javascript Event Loop + Angular Context (everything going on inside AngularJS)
If you add something to the scope, AngularJS is smart enough to keep track of that variable
and so it adds watchers.

Watchers (old value -> new value) 
for every variable, function or anything on html page, AngularJS adds a watcher to the list

To keep track of this watching for old and new, AngularJS makes use of Digest loop or cycle like
the event loop but this is an Angular looTo keep track of this watching for old and new, 
AngularJS makes use of Digest loop or cycle like, the event loop but this is an Angular 
loop.

When you enter into a digest loop, it checks every variable for its old and new value and if anything
has changed - then it updates everywhere that is connected to it in the DOM. It runs again to check
if all old values and new values are matched. This is also the missing bit between model and view in 
AngularJS.

## ng-cloak

Hides an element in the dom until AngularJS has worked on it
```
<div ng-cloak>{{ name }}</div>
```
Available directives: https://docs.angularjs.org/api/ng/directive

## XMLHTTPRequest Object
An object capable of making internet requests on its own,
API calls inside our application, but you need to use the Angular wrapper

```
$scope.$apply(function() {
 var rulesreq = new XMLHttpRequest();
 rulesreq.onreadystatechange = function () {
 }
}
```
However, it is preferred to do it the Angular way,

```
myApp.controller('mainController', ['$scope', '$filter', '$http', function ($scope,
$filter, $http) {
    $scope.handle = '';
    $scope.lowercasehandle = function () {
        return $filter('lowercase')($scope.handle);
    };
    $scope.characters = 5;
    $http.get('/api')
         .success(function (result) {
         
         })
         .error(function (data, status) {
             console.log(data);
         })
}
```

## Multiple Controllers and multiple views

```
myApp.controller('istcontroller',....)
myApp.controller('seccontroller',....)
```

## Multiple URLs and single page applications
Using fragment identifier in single page applications
You just download one page and then you use asynchronous
requests via ajax or browser requests using fragment 
identifier, each hash value corresponds to a page

```
window.addEventListener('hashchange', functuon() {
     if (window.location.hash === '#/bookmark/1') {
         console.log('Page 1 is cool');
     }
     if (window.location.hash === '#/bookmark/2') {
         console.log('Let me go get Page 2.');
     }
    
     if (window.location.hash === '#/bookmark/3') {
         console.log('Here is Page 3');
     }
});
```

Angular can also keep track of your location and hence
use a single page app to deliver a multiple page functionality

```
// Show location, address bar in console.log
// AngularJS knows what the hash is
myApp.controller('mainController', ['$scope', '$location', '$log',
function($scope, $location, $log) {
      $log.info($location.path());
}]);
```

Angular builtin: angular-route.js which is a router
