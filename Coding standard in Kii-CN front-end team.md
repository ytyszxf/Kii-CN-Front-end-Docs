# Coding standard in Kii-CN front-end team
## Table of Contents
1. [Javascript Coding Standard](#code-standard)
2. [Angular JS](#angular-js)
3. [SCSS](#scss)
4. [HTML](#html)
5. [Front-end Project Framework](#front-end-framework)

<a name="code-standard"></a>
### 1 Coding Standard
ES5: We reference Airbnb's Javascript ES 5/6 coding style standard as our coding standard. Below links will guide you to the code style detail.


[ES 5](https://github.com/airbnb/javascript/tree/master/es5); [ES 6](https://github.com/airbnb/javascript/blob/master/README.md)

Recommand IDE: Sublime 3, Webstorm. Just a recommandation.

### `Something that is different and IMPORTANT`
 - 1.1 semi-colon
Remember to put semi-colon after every line of code, except function declaration.

```javascript

// bad
// This will mass code up when compress javascript file. Fixing this type of issue is very time comsuming.
var k = 1
var j = 2

// good
var k = 1;
var f = function(){
    //...
};

function m (){
}

```
 - 1.2 Indent
We use 4 white space as indent instead of 2. `WARNING` DONT USE tab as indent. Some IDE support replace tab indence with white space. BUT in html and css we will use 2 white space. Please follow this rule strictly.

    For sublime, [FYI](https://www.sublimetext.com/docs/3/indentation.html).

 - 1.3 Constant Variable
Please put comprehensive and easy understanding comments near constant variable declaration. For most of case constant variable should be wrapped in a class or a config file.
    
 - 1.4 if statement
Be careful of using if(a) when you cannot ensure the type of a. undefined, false, 0, null will give you same result, but sometimes it is not what you expected. 


```javascript
function Car(){
    this.speed = null;
}

Car.prototype.setSpeed = function(speed){
    if(!speed){
        throw('speed is undefined');
    }
    this.speed = speed;
};

var car = new Car();
var speed = 0;

// this will throw exception, but it is not what we expect.
car.setSpeed(speed);
```
 - 1.5 Underscore Lib
 Use underscore lib as much as possible, that will save you a lot of code, and much easier to read. FYI, [underscore](http://underscorejs.org/).

```javascript
    var k = [1, 2, 3];
    var funcs = [];
    
    // bad
    for(var i = 0; i < k.length; i++){
        var myFunc = function(myIndex){
            return function(){
                console.log(myIndex);
            }
        }(k[i]);
        funcs.push(myFunc);
    }
    
    // good
    _.each(k, function(myIndex){
        funcs.push((x) = > {console.log(myIndex)});
    });
```
 - 1.6 Event
 Remember to unbind event when this event is not used any more. This type of issue happens very often in single-page application and always leaves a lot of e
xceptions log in console. Sometimes it has no negative effect on the program, but please DO NOT leave it there.


<a name="angular-js"></a>
### 2 Angular JS
 - 2.1 Directives
 Always using object to interact with directive. This is very important. If you don't do like this, you will lose binding in nested directive.
```javascript 
angular.module('myApp').directive('myDirective',
  function(){
    return {
        restrict: 'E',
        scope: {
            myModel: '=?'
        },
        link: function(scope) {
            
        }
    };
  });
  
 angular.module('myApp').controller('myController', ['$scope', function($scope){
    $scope.myObject = {
        aModel: 't'
    };
 }]);
```

```html
<my-directive my-model="myObject.aModel"></mydirective>
```
 - 2.2 Module injections
 This rule should be followed strictly. Please read examples below.
```javascript
// bad
angular.module('myApp').controller(function($scope, $myModuleInstance){
   // ... 
});

// good
angular.module('myApp').controller(['$scope', '$myModuleInstance', function($scope, $myModuleInstance]){
   // ... 
});

// bad
$uiModal.open({
    // ...
    controller: function($scope){
    }
});

// good
$uiModal.open({
    // ...
    controller: ['$scope', function($scope){
    }]
});
```
 - 2.3 router
  Always use ```ui.router```. [FYI](https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwi_3NrBx5fMAhWFX5QKHaptA0kQFggbMAA&url=https%3A%2F%2Fgithub.com%2Fangular-ui%2Fui-router&usg=AFQjCNGGenLMwsnWDz8t-pkIQlSLFjSb1A&cad=rja)
 - 2.4 storage
  Always use angular storage for ```session/local storage```. [FYI](https://github.com/grevory/angular-local-storage)
 - 2.5 DOM
  Avoid doing operation on dom object directly as the best you can do. Using original Javascript code or JQuery or other lib except angularJS to add/delete/update dom element will mess angular's life circle. Thi should be strickly avoided.

 - 2.5 HTTP
  Use ```$http``` for any request. ```jQuery.ajax``` or other libs are not in angular's life circle, so though this is not a MUST, use ```$http``` as much as you can.
 - 2.6 Constant
 All constant should be declared in ```angular.constant```.
```javascript
angular.module('myApp').constant('AuthenticExceptions', {
    LOGIN_SUCCESS: 'LOGIN_SUCCESS',
    TOKEN_EXPIRE: 'TOKEN_EXPIRE',
    // ...
});
```
- 2.7 App Initialization
```javascript
angular.module('myApp').run(
  ['$rootScope', '$state', '$stateParams', 'AppUtils',
      function($rootScope, $state, $stateParams, AppUtils) {
      
          $rootScope.$state = $state;
          $rootScope.$stateParams = $stateParams;
          /* 
           * init AppUtils
           */
          AppUtils.initialize();
          window.AppUtils = AppUtils;
      }
  ]
);
```
### 3 SCSS
 For scss we also use [airbnb's coding style](https://github.com/airbnb/css) as reference.

 Additionaly, we extend it as below:
 
 - 3.1 Indent
  Use 2 white space rule.

### 4 HTML
 - 4.1 Indent
 Use 2 white space rule.

### 5 Front-end Project Framework
The project can be downloaded from [here]( https://github.com/ytyszxf/front-end-template). 

 - 5.1 Project Installation
Refer to project [readme file]( https://github.com/ytyszxf/front-end-template/tree/master/README.md).

 - 5.2 Project Structure
Our front-end project will always be made up of two parts, ```client``` and ```server```, which is just like showed below.

 ```
 |-- client--|-- gulp
 |           |-- bower_components
 |           |-- src
 |           |-- .tmp
 |
 |-- sever --|-- bin
 |           |-- app.js
 
 ```
 
 The ```client``` folder contains everything that displays on browser.
 The ```server``` part is just a light server launcher for development usage, but you can also use it as a simple backend in production environment.
 
- 5.2.1 Gulp
    The tool ```Gulp``` aims to automate and enhance  workflow. In our case, we use it to help us with some tasks including: ```partials```, ```javascript injection```, ```angular html cache```, ```font injection```, ```compile sass```, ```compress files```.
We define gulp default setting in ```client/gulpfile.js```. Tasks are defined in ```client/gulp/*.js```. In this folder, we have ```buiding.js```, ```inject.js``` and ```watch.js```. ```watch.js``` defines tasks that watch some files. When those file changes, building process are fired. ```building.js``` and ```inject.js``` helps with building process. You might be very confused why front-end project need building process. Below is our ```index.html```. In our case, this is a single-page web project, which means it is the only entrance of our app and we put all javascript and css here. For a project, the number of files we need to included in ```index.html``` will be significant, and very hard to maintain. So we run script to autimate this process to inject all libs and our own files. Gulp is very handy as a building tool. 

```html
<!doctype html>
<html class="no-js" ng-app="myApp">

<head>
  <meta charset="utf-8">
  <title>Street Lights</title>
  <meta name="description" content="">
  <meta name="viewport" content="width=device-width">
  <!-- Place favicon.ico and apple-touch-icon.png in the root directory -->
  <!-- build:css({.tmp/serve,src}) styles/vendor.css -->
  <!-- bower:css -->
  <!-- run `gulp wiredep` to automaticaly populate bower styles dependencies -->
  <!-- endbower -->
  <!-- endbuild -->
  <!-- build:css({.tmp/serve,src}) styles/app.css -->
  <!-- inject:css -->
  <!-- css files will be automaticaly insert here -->
  <!-- endinject -->
  <!-- endbuild -->
</head>

<body class="fixed">
  <!-- [if lt IE 6]>
      <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
    <![endif] -->
  <div ui-view></div>
  <!-- build:js(src) scripts/vendor.js -->
  <!-- bower:js -->
  <!-- run `gulp wiredep` to automaticaly populate bower script dependencies -->
  <!-- endbower -->
  <!-- endbuild -->
  <!-- build:js({.tmp/serve,.tmp/partials,src}) scripts/app.js -->

  <!-- app:js -->
  <!-- js files will be automaticaly insert here -->
  <!-- endinject -->

  <!-- inject:js -->
  <!-- js files will be automaticaly insert here -->
  <!-- endinject -->

  <!-- route:js -->
  <!-- js files will be automaticaly insert here -->
  <!-- endinject -->

  <!-- inject:partials -->
  <!-- angular templates will be automatically converted in js and inserted here -->
  <!-- endinject -->
  <!-- endbuild -->
</body>

</html>
```
 
 **Be Careful!** gulp inject bower project into app by reading ```bower.js``` of bower project. Let's take ```Bootstrap``` as an example. Below is the directory structure and ```bower.js``` file of bootstrap. Gulp reads main node, and get the file path. The main node does not include ```"dist/css/bootstrap.css"```, so if you don't add it in ```bower.js```, your ```index.html``` will not include ```bootstrap.css```. So if you find out that some of your libs is not working, go and check this first.
 
 **Directory Structure**
```javascript
 |-- dist
 |-- fonts
 |-- bower.json
 |-- ...
 |-- ...
```

**bower.js**

```json
{
  "name": "bootstrap",
  "description": "The most popular front-end framework for developing responsive, mobile first projects on the web.",
  "keywords": [
    "css",
    "js",
    "less",
    "mobile-first",
    "responsive",
    "front-end",
    "framework",
    "web"
  ],
  "homepage": "http://getbootstrap.com",
  "license": "MIT",
  "moduleType": "globals",
  "main": [
    "less/bootstrap.less",
    "dist/js/bootstrap.js"
  ],
  "ignore": [
    "/.*",
    "_config.yml",
    "CNAME",
    "composer.json",
    "CONTRIBUTING.md",
    "docs",
    "js/tests",
    "test-infra"
  ],
  "dependencies": {
    "jquery": "1.9.1 - 2"
  }
}

```
 
 - 5.3 Bower
 In our project, we don't copy any lib into our project if we can find it in ```bower```. Take ```bootstrap``` as an example, we put it into our project by running ```$ bower install bootstrap --save```. If you need some libs that are only needed in development rather than in production environment, you should run  ```$ bower install bootstrap --save-dev```. Try to make our project **CLEAN**.

 - 5.4 App Structure
 Below is app source structure.
 ```
 |-- app--|-- components--|-- AppShared
 |        |               |-- Secure
 |        |               |-- ...
 |        |               
 |        |-- app.html
 |        |-- app.controller.js
 |        |-- app.js
 |        |-- app.scss
 |        |-- app.route.js
 |
 |-- fonts
 |-- images
 |-- css
 |-- favicon.ico
 |-- index.html
 ```
  - 5.4.1 Module Files Naming Rule
  Create a new module must follow several rules. Let's say we are going to create a ```User Management``` module. We will create a folder under ```app/components``` named ```UserManager```(capitalize every words and remove blanks). We will have several files added into this folder, including: ```UserManager.js```, ```UserManager.controller.js```, ```UserManager.html```, ```UserManager.html```, ```UserManager.scss```. If you need to create service, name it as ```UserManager.service.js```. If you have sub module, for instance, you have sub module ```NewUser```, you will create ```UserManager.router.js```, and put ```NewUser``` in its router config. Then the structure will be like this:
```
 |-- app--|-- components--|-- AppShared
 |        |               |-- Secure
 |        |               |-- UserManager --|-- NewUser
 |        |                                 |-- UserManager.js
 |        |                                 |-- UserManager.controller.js
 |        |                                 |-- UserManager.service.js
 |        |                                 |-- UserManager.router.js
 |        |                                 |-- UserManager.html
 |        |                                 |-- UserManager.scss
 |        |-- app.html
 |        |-- app.controller.js
 |        |-- app.js
 |        |-- app.scss
 |        |-- app.route.js
```

  - 5.4.2 Scss and HTML Coding Rule
  Continue previous example, in ```UserManager.scss```, the first line should always be ```@import 'src/app/components/AppShared/scss/global.scss'```. ```global.scss``` is a global mixin lib, so you should use it as much as you can to keep the web page style uniformed. And the second line is the module's parents' name + '-' + module name. All in lower case and split by ```-```. The major benifit is it will keep the changes in your module and its sub modules, and prevent you from mess up the global style.
```css
@import 'src/app/components/AppShared/scss/global.scss';
.app-usermanager{
    div{
        background: red;
    }
}
```
So, for ```NewUser``` module, its ```NewUser.scss``` should be:
```css
@import 'src/app/components/AppShared/scss/global.scss';
.app-usermanager-newuser{
    
}
```

In HTML, accordingly, you should add the class name, like the following two blocks. Remember to write comments and keep the code clean.
**UserManager.html**
```html
<div class="app-usermanager" ng-init="init()">
  <!-- Sub State -->
  <div ui-view>
  </div>
  <!-- End of Sub State-->
</div>
```

**NewUser.html**
```html
<div class="app-usermanager-newuser" ng-init="init()">
  <form ng-submit="onRegisterSubmit(newuser)">
    <!-- start username -->
    <div class="form-gorup">
        <label>User Name</label>
        <input type="text" class="form-control" ng-model="newuser.username"/>
    </div>
    
    <!-- start password -->
    <div class="form-gorup">
        <label>Password</label>
        <input type="text" class="form-control" ng-model="newuser.password"/>
    </div>
    
    <!-- start submit button -->
    <div class="form-gorup">
        <button class="btn btn-success" type="submit"></button>
    </div>
  </form>
</div>
```

 - 5.4.3 Directives
 Continue previous example, if we are going to add a new directive ```name-validate``` for ```NewUser``` module, we should add a folder named ```directives``` under ```NewUser```, and add a folder ```name-validate``` under ```directives``` folder. In this folder, we create three files: ```name-validate.directive.js```, ```name-validate.template.html```, ```name-validate.scss```. The directory structure will be like folloing:
```
|-- NewUser--|-- directives--|name-validate --|-- name-validate.directive.js
|            |                                |-- name-validate.template.html
|            |                                |-- name-validate.scss
|            |
|            |-- NewUser.js
|            |-- NewUser.controller.js
|            |-- ..
```
If the directive is used in whole app scope, you should put it under ```AppShared/directives/name-validate```.

