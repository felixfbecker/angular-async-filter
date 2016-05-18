
[![Version](https://img.shields.io/npm/v/angular-async-filter.svg?maxAge=2592000)](https://www.npmjs.com/package/angular-async-filter)
[![Downloads](https://img.shields.io/npm/dt/angular-async-filter.svg?maxAge=2592000)](https://www.npmjs.com/package/angular-async-filter)
[![Build Status](https://travis-ci.org/felixfbecker/angular-async-filter.svg?branch=master)](https://travis-ci.org/felixfbecker/angular-async-filter)
![Dependencies](https://david-dm.org/felixfbecker/angular-async-filter.svg)
[![License](https://img.shields.io/npm/l/angular-async-filter.svg?maxAge=2592000)](https://github.com/felixfbecker/angular-async-filter/blob/master/LICENSE.md)


In TodoMVC examples all data may be local, but in _real_ applications data is often fetched async from a remote.
Before Angular 1.3, Angular _implicitly_ unwrapped any promise in the scope to its resolved value.
But in 1.3, it was removed.
In Angular 2, you can _explicitly_ await an async value with the `async` pipe.
That way you can bind promises to your controller and only show the value in the view.
This works pretty well because Angular's $q promises are integrated with the $digest loops, which means the view will be updated when a promise is resolved.

To implement this in Angular 1 without triggering an infinite $digest loop a `WeakMap` of promises is kept by the filter.
WeakMaps are supported in all modern browsers. If you're targeting older browsers, you can include a [WeakMap Polyfill](https://www.npmjs.com/package/weakmap).

Supports browserify/webpack.

## Example
```js
angular.module('myApp', ['async'])
  .controller('TestController', function ($http) {
    this.products = $http.get('/api/products').then(response => response.data)
  })
```
```html
<ul ng-controller="TestController as ctrl">
  <li ng-repeat="product in ctrl.products | async">
    {{product.name}}, {{product.price}}€
  </li>
</ul>
```

## Benefits
 - You don't have to add a `.then` handler that assigns the value to the controller
 - You can access (and await) the promise in your controller methods without storing it _in addition_ to the value

## States
<table>
  <tr><td>pending</td><td><code>undefined</code></td></tr>
  <tr><td>fulfilled</td><td>resolved value</td></tr>
  <tr><td>rejected</td><td><code>undefined</code></td></tr>
</table>
