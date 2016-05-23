# Bloomfire JS Styleguide

* This is the Javascript styleguide that we use at Bloomfire engineering. It is
based on [Airbnb's wonderful styleguide] and uses some of its examples.

## Table of Contents

  1. [Strings](#strings)
  1. [Objects](#objects)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Constructors](#constructors)
  1. [Events](#events)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [Underscore](#underscore)
  1. [Contributors](#contributors)
  1. [React](./react.md)
  1. [RxJS](./rxjs.md)

## Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    var name = "Bad Mc'Baderson";

    // good
    var name = 'Gabe Hernandez';

    // bad
    var fullName = "Bad " + this.lastName;

    // good
    var fullName = 'Gabe ' + this.lastName;
    ```

  - Strings longer than 80 characters should be written across multiple lines
  using string concatenation.

  - Use Array.join when dynamically generating a string.

    ```javascript
    var data = [
      {name: 'Gabe', id: 1},
      {name: 'James', id: 2},
      {name: 'Bishop', id: 3}
    ];

    // bad
    var authorString = 'The authors are: ';
    _.each(data, function (author) {
      authorNames += author.name;
    });

    // good
    var authors = [];
    _.each(data, function (author) {
      authors.push(author.name);
    });
    var authorString = 'The authors are: ' + authors.join('');
    ```

  **[⬆ back to top](#table-of-contents)**

## Objects

  - Do not use wrap quotes around object key names.

  ```javascript

  // bad
  var post = {
    "title": "Dont do it this way."
  };

  // good
  var post = {
    title: 'Do it this way instead.'
  }
  ```

## Functions

  - Never declare a function in a non-function block (if, while, etc). Assign
  the function to a variable instead. Browsers will allow you to do it, but they
  all interpret it different.

    ```javascript
    // bad
    if (user) {
      function logUser (name) {
        console.log('Don not do this.');
      }
      logUser(user.name);
    }

    // good
    var logUser = function (name) {
      console.log('Do this.');
    };
    if (user) {
      logUser(user.name);
    }
    ```

  - Keep functions short and specific. They should only do a single thing. A
  good rule of thumb is if you find yourself writing a function or method more
  than 8-10 lines, it can most likely be broken up into 2 or more methods.
  (Remember, the computer doesn't need code to be readable but you and other
  developers do.)

    ```javascript

    // bad.
    // (Difficult to skim and have an understanding of whats happening.)
    var renderAllUsers = function (data) {

      var users = _.map(data, function (user) {
        user.fullName = user.firstName + user.lastName;
        return user;
      });

      _.each(users, function (user) {
        var userOptions = _.extend(user, {
        context: this.$('body')
        });
        var userView = new UserView(userOptions);
      }, this);
    };


    // good.
    // (Easier to digest and see whats going on at a glance.)
    var normalizeUsers = function (users) {
      var normalizedUsers = _.map(users, function (user) {
        user.fullName = user.firstName + user.lastName;
        return user;
      });
      return normalizedUsers;
    };

    generateUserViewOptions = function (userData) {
      options = _.extend(userData, {
        context: this.$('body')
      });
      return options;
    };

    var renderUser = function (userData) {
      var userOptions = generateUserViewOptions(userData);
      var userView = new UserView(userOptions);
    },

    var renderAllUsers = function (data) {
      var users = normalizeUsers(data);
      _.each(users, function (user) {
        renderUser(user);
      });
    };
    ```

  **[⬆ back to top](#table-of-contents)**

## Properties

  - Access object properties with dot notation.

    ```javascript
    var Nick = {
      isWeatherMan: true,
      isGoodAtPingpong: false
    };

    // bad
    var isWeatherman = Nick['isWeatherMan'];

    // good
    var isGoodAtPingpong = Nick.isGoodAtPingpong;
    ```

  **[⬆ back to top](#table-of-contents)**

## Variables

  - Always use `var` to declare variables.

    ```javascript
    // bad
    view = new View();

    // good
    var view = new View();
    ```

  - Use one `var` declaration per variable. It's easier to add new variable
  declarations this way, and you never have to worry about swapping out a ; for
  a , or introducing punctuation-only diffs.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Declare unassigned variables last. This is helpful when later on you might
  need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - It is not necessary to assign variables at the top of their scope but do try
  to define them close to the code that acts on them.

    ```javascript
    //bad
    function () {
      console.log('log');
      console.log('another log');

      // ..more code

      // This could be difficult to find amongst all the other code.
      var user = _.findWhere({name: 'Gabe'});

      console.log('log');
      console.log('another log');

      // ..more code

      user.age = 26;
      return user;

    }

    // good
    function () {
      console.log('log');
      console.log('another log');


      // ..more code..

      var user = _.findWhere({name: 'Gabe'});
      user.age = 26;
      return user;
    }
    ```

  **[⬆ back to top](#table-of-contents)**

  # Comparison Operators & Equality

  - Use `===` and `!==` over `==` and `!=`.

  - Make use of underscore/lodash very useful utility comparison & equality
  methods. These normalize behavior across browsers and takes care of some weird
  JS gotchas that pop up. They can be found in the underscore docs under [object methods]
  > isEmpty()
  > isElement()
  > isArray()
  > isObject()
  > isFunction()
  > isUndefined()
  > isNull()
  > etc

    ```javascript
    // bad
    var users = [];

    if (typeof users === 'object') {

      // even though 'users' is an array, in javascript arrays are really objects.
      // This code will run when you don't expect it to.
      console.log('users is an object');
    } else {
      console.log('users is an array');
    }


    // good
    if (_.isObject(users)) {
      console.log('users is an object');
    } else {

      // This is the code we expect to run and will do so correctly.
      console.log('users is an array');
    }
    ```

  - Use shortcuts when possible.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }

    // bad
    return isCollection ? true : false;

    // good
    return isCollection;
    ```

  **[⬆ back to top](#table-of-contents)**

## Blocks

  - Use braces with all multi-line blocks. It is ok to place very simple if
  conditional and statement on the same line (usually when you want to assign or
  return a single value)

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

  - If you're using multi-line blocks with if and else, put else on the same
  line as your if block's closing brace.

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

  **[⬆ back to top](#table-of-contents)**


## Comments

  - Use `/** ... */` for multi-line comments. Include a description, specify types
  and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */

    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use // for single line comments. Place single line comments on a newline
  above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Make use of comments that are prefixed with `TODO: ` and `NOTE: ` followed
  by a useful description. Use `TODO` to record a task still needing completion.
  Use `NOTE` when you need to give a deeper explanation or want to point out a
  certain nuance or subtlety with a piece of code. These two type of comments
  can help other developers get a better idea of the state and purpose of your
  code.

    ```javascript
      // TODO: This data needs to eventually come from the API.
      var data = [
        {name: 'Gabe'},
        {name: 'James'},
        {name: 'Chris'}
      ];

      // NOTE: the ?fields=id query sting is a temporary fix for our backend
      // expecting a field called `content_type` param. We dont really care
      // about that for a mention so if we just ask for the id in the fields
      // param then it skips that. It is a hack to work with our unfriendly
      // backend.
      var fieldsQueryString = '?fields=id';

      if (this.get('followed')) {
        return followRoot + '/' + this.get('id') + fieldsQueryString;
      } else {
        return followRoot + fieldsQueryString;
      }
    ```

  **[⬆ back to top](#table-of-contents)**

## Whitespace

  - Use spaces and set to 2 spaces

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙var name;
    }
    ```

  - Place 1 space before the leading brace. Also put a space after a
  function decleration name and the `(` opening parenthese that starts the
  arguments.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // bad. No space after name 'test'
    function test() {
      console.log('test');
    }

    // good
    function test () {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Place 1 space before the opening parenthesis in control statements (if,
  while etc.). Place no space before the argument list in function calls.

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight () {
      console.log('Swooosh!');
    }
    ```

  - Set off operators with spaces.


    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a blank line.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

  - Use indentation when making long method chains. Use a leading dot, which
  emphasizes that the line is a method call, not a new statement.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
      .highlight()
      .end()
      .find('.open')
      .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
        .enter()
        .append('svg:svg')
        .classed('led', true)
        .attr('width',  (radius + margin) * 2)
        .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - Leave a blank line after blocks and before the next statement

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```

  - As much as possible, keep your lines of code under 80 charaters long. This
  forces you to keep your code readable, concise, and maintainable. This may
  seem like an unnecessary restriction, but you will finding yourself
  starting to write more readable code (remember, give it 5 minutes).

  ```javascript

  // bad.
  // Hard to read.
  var embedlyUrl = 'https://api.embed.ly/1/preview?wmode=opaque&key=' + ContentConfig.upload.embedlyKey + '&url=' + encodeURIComponent(embed_url);

  // better
  var embedlyUrl = 'https://api.embed.ly/1/preview?wmode=opaque&key=' +
        ContentConfig.upload.embedlyKey + '&url=' +
        encodeURIComponent(embed_url);

  // best
  var embedlyRoot = 'https://api.embed.ly/1/preview'
  var queryParams = '?wmode=opaque&key=' + ContentConfig.upload.embedlyKey +
    '&url=' + encodeURIComponent(embed_url);
  var embedlyUrl = embedlyRoot + queryParams;

  // ok
  // This is a bit fine grained but still acceptable.
  var embedlyRoot = 'https://api.embed.ly/1/preview';
  var wmodeParam = '?wmode=opaque';
  var keyParam = '&key=' + ContentConfig.upload.embedlyKey;
  var urlParam = '&url=' + encodeURIComponent(embed_url);
  var queryParams = wmodeParam + keyParam + urlParam;
  var embedlyUrl = embedlyRoot + queryParams;

  ```

  - When breaking to a new line where there are multiple conditionals, do so
  with the ending `&&` or `||` on a previous line.

  ```javascript

    // bad
    if (something && somethingElse
        && theFinalThing)

    // good
    if (something && somethingElse &&
        theFinalThing)

  ```


  **[⬆ back to top](#table-of-contents)**

## Commas

  - Do not use leading commas

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime
    ];

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };

    // bad
    var ejs = require('ejs')
    , express = require('express')
    , cookieParser = require('cookieParser')
    , bodyParser = require('bodyParser');

    // good
    var ejs = require('ejs');
    var express = require('express');
    var cookieParser = require('cookieParser');
    var bodyParser = require('bodyParser');
    ```

  - Remove all unneccessary trailing commas.

    ```javascript
    // bad
    var data = [
      {name: 'Gabe'},
      {name: 'Nick'},
    ];

    // bad
    var names = [
      'Gabe',
      'Nick',
    ];

    // good
    var data = [
      {name: 'Gabe'},
      {name: 'Nick'}
    ];

    // good
    var names = [
      'Gabe',
      'Nick'
    ];
    ```

  **[⬆ back to top](#table-of-contents)**

## Semicolons

  - **Always** use semicolons

    ```javascript
    // bad
    function () {
      var name = 'Skywalker'
      return name
    }

    // bad
    if (isJedi) return true

    // good
    function () {
      var name = 'Skywalker';
      return name;
    }

    // good
    if (isJedi) return true;
    ```

  **[⬆ back to top](#table-of-contents)**

## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming. The only
  exception is with the iteration number argument `i` in loops.

    ```javascript
    // bad
    function q() {
      // ..stuff..
    }

    // good
    function query() {
      // ..stuff..
    }

    // bad
    _.each(collection, function (m) {
      // ..stuff..
    });

    // good
    _.each(collection, function (model) {
      // ..stuff..
    });

    // good
    _.each(collection, function (model, i) {
      // ..stuff..
    });
    ```

  - Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c () {}
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction () {}
    var userView = new UserView({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes.

    ```javascript
    // bad
    function user (options) {
      this.name = options.name;
    }

    var bad = new userView({
      name: 'nope'
    });

    var commentModel = Backbone.Model.extend({});

    // good
    function User (options) {
      this.name = options.name;
    }

    var good = new UserView({
      name: 'yup'
    });

    var CommentModel = Backbone.Model.extend({});
    ```

  - When saving a reference to this use `self`.

    ```javascript
    // bad
    function () {
      var that = this;
      return function () {
        console.log(that);
      };
    }

    // bad
    function () {
      var _this = this;
      return function () {
        console.log(_this);
      };
    }

    // good
    function () {
      var self = this;
      return function () {
        console.log(self);
      };
    }
    ```

  - Name your functions as much as possible. This is helpful for stack traces.

     ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - If your file exports a single class, your filename should be exactly the
  name of the class.

    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // in some other file
    // bad
    var CheckBox = require('./checkBox');

    // bad
    var CheckBox = require('./check_box');

    // good
    var CheckBox = require('./CheckBox');
    ```

  **[⬆ back to top](#table-of-contents)**

## jQuery

  - Most ajax calls should be abstracted out into a method but when you do need
  to write an ajax request, be explicit and use `$.ajax` with a configuration
  object as its argument exclusively.

    ```javascript

    // bad
    $.get('some/url', function (res) {
      // ... do stuff ...
    });

    // bad
    $.getJSON('some/url', function (res) {
      // ... do stuff ...
    });

    // bad
    $.post('some/url', data, function (res) {
      // ... do stuff ...
    });

    // bad
    $.ajax('some/url', {
      method: 'POST',
      data: postData
      success: function (res) { // ... do stuff ...},
      error: function (jqXHR, status, err) { // ... do stuff ...}
    })

    // good
    $.ajax({
      url: 'some/url',
      method: 'POST',
      data: postData,
      success: function (res) { // ... do stuff ...},
      error: function (jqXHR, status, err) { // ... do stuff ...}
    });

    ```

  - When defining success, error, and complete callbacks in your ajax
  options, do so in that order (i.e. success first, error second, complete last).
  This one is purely to keep a consistant styling to the code.

    ```javascript

    // bad
    $.ajax({
      url: 'some/url',
      error: function () {},
      success: function () {},
      complete: function () {}
    });

    // good
    $.ajax({
      url: 'some/url',
      success: function () {},
      error: function () {},
      complete: function () {}
    });

    ```



  **[⬆ back to top](#table-of-contents)**

## Underscore

  - When possible, always use underscore methods over jquery methods.

    ```javascript

    // bad
    $.each(elements, function (i, el) {
      // do stuff
    });

    // bad
    var object = $.extend({}, {
      library: 'jquery'
    });

    // good
    _.each(elements, function (el, i) {
      // do stuff
    });

    // good
    var object = _.extend({}, {
      library: 'underscore'
    })
    ```

  - The only exception to that rule is when you need to deep copy an object (
  which underscore doesnt support)

    ```javascript

    // ok
    var userCopy = $.extend(true, user, {
      age: 26
    });
    ```

  - A lot of underscore methods accept a third context argument. Use that
  argument instead of assigning and using a `self` variable.

    ```javascript

    // bad
    var self = this;
    _.each(comments, function (comment) {
      self.displayComment();
    });

    // good
    _.each(comments, function (comment) {
      this.displayComment();
    }, this);
    ```

  **[⬆ back to top](#table-of-contents)**



[Airbnb's wonderful styleguide]:https://github.com/airbnb/javascript
[object methods]:http://underscorejs.org/#objects
