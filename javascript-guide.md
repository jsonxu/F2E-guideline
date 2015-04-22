[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/airbnb/javascript?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Airbnb JavaScript Style Guide
# 前端JavaScript编码规范
*JavaScript最佳通用规范*


## 内容提纲

  1. [类型](#types)
  1. [对象](#objects)
  1. [字符串](#strings)
  1. [数组](#arrays)
  1. [函数](#functions)
  1. [对象的属性](#properties)
  1. [变量](#variables)
  1. [Hoisting](#hoisting)封装？
  1. [条件语句和等号运算符](#comparison-operators--equality)
  1. [代码块](#blocks)
  1. [注释](#comments)
  1. [空格](#whitespace)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [命名约定](#naming-conventions)
  1. [存取器](#accessors)
  1. [构造器](#constructors)
  1. [事件](#events)
  1. [模块](#modules)
  1. [jQuery](#jquery)
  1. [ECMAScript5 兼容性](#ecmascript-5-compatibility)
  1. [性能](#performance)
  1. [资源](#resources)
  1. [现状](#in-the-wild)
  1. [翻译](#translation)
  1. [JavaScript风格指南](#the-javascript-style-guide-guide)
  1. [请与我们交流关于JavaScript](#chat-with-us-about-javascript)
  1. [贡献者](#contributors)
  1. [许可声明](#license)

## 类型

  - **原始值**: 当你使用一个变量的原始值，赋值到新变量时

    + `string字符串`
    + `number数字型`
    + `boolean布尔型`
    + `null空值`
    + `undefined不存在型`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **复杂类型**: 当你把一个数组赋值到新变量上并改变数组内成员的值

    + `object对象`
    + `array数组`
    + `function函数`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到顶部](#table-of-contents)**

## 对象

  - 使用字面量的方法创建对象

    ```javascript
    // 不推荐
    var item = new Object();

    // 推荐
    var item = {};
    ```

  - 不要使用 [javascript保留关键字](http://es5.github.io/#x7.6.1)作为key值. IE8不支持. [更多参考信息](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // 不推荐
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // 推荐
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - 使用意义接近的单词替代保留关键字

    ```javascript
    // 不推荐
    var superman = {
      class: 'alien'
    };

    // 不推荐
    var superman = {
      klass: 'alien'
    };

    // 推荐
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ 回到顶部](#table-of-contents)**

## 数组

  - 使用字面量的方法创建对象

    ```javascript
    // 不推荐
    var items = new Array();

    // 推荐
    var items = [];
    ```

  - 使用数组对象中push的方法替代直接增加数组成员的方法
    ```javascript
    var someStack = [];


    // 不推荐
    someStack[someStack.length] = 'abracadabra';

    // 推荐
    someStack.push('abracadabra');
    ```

  - 当你需要复制一个数组时请使用slice方法 [参考](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // 不推荐
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // 推荐
    itemsCopy = items.slice();
    ```

  - 使用slice方法将类似数组一样的对象转换成数组
    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 字符串

  - 用单引号 `''` 创建字符串.

    ```javascript
    // 不推荐
    var name = "Bob Parr";

    // 推荐
    var name = 'Bob Parr';

    // 不推荐
    var fullName = "Bob " + this.lastName;

    // 推荐
    var fullName = 'Bob ' + this.lastName;
    ```

  - 建议字符长度超过80的字符串，拆分成小于80长度的字符串，使用字符串拼接的方法拼接
  - 提示: 如果大量使用长字符串拼接，会对性能造成影响 [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // 不推荐
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // 不推荐
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // 推荐
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - 建议通过使用join方法来连接字符串，而不要直接将字符串拼接，特别是在IE下性能比较低: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // 不推荐
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // 推荐
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 函数

  - 函数表达式:

    ```javascript
    // 匿名函数表达式
    var anonymous = function() {
      return true;
    };

    // 有名函数表达式
    var named = function named() {
      return true;
    };

    // 自调用函数表达式
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - 绝对不要在非函数内直接定义一个函数，建议你将定义的函数赋给一个变量，虽然两种做法都可行，但在浏览器里解析是不一样的。
  - **注意:** ECMA-262 定义 `块` 型代码为一组一句. 函数声明不是一个语句. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // 不推荐
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // 推荐
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - 绝对不要将参数命名为 `arguments`，在作用域内的其他函数传参会有问题

    ```javascript
    // 不推荐
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // 推荐
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**



## 对象的属性

  - 使用 `.` 表示法来访问对象的成员.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // 不推荐
    var isJedi = luke['jedi'];

    // 推荐
    var isJedi = luke.jedi;
    ```

  - 当对象的键是参数变量的时候，请使用 `[]`的表示法访问对象的成员
    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 变量

  - 一定要使用 `var` 来声明变量.否则会导致全局变量污染.应该避免这样对全局命名空间污染.

    ```javascript
    // 不推荐
    superPower = new SuperPower();

    // 推荐
    var superPower = new SuperPower();
    ```

  - 使用一个 `var` 只声明一个变量, 你绝对不用担心把 逗号`,`和分号`;`张冠李戴了.


    ```javascript
    // 不推荐
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // 不推荐
    // (比较上面的代码，范了个错误)
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // 推荐
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - 将未赋值的变量放在末尾，当需要用到前面已赋值变量就会很有帮助。
    ```javascript
    // 不推荐
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // 不推荐
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // 推荐
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - 把变量声明在它的作用域顶部，有助于避免变量声明和赋值产生相关问题。
    ```javascript
    // 不推荐
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // 推荐
    function() {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // 不推荐
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // 推荐
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## Hoisting

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ 回到顶部](#table-of-contents)**



## 条件语句和等号运算符

  - 合理使用 `===` 和 `!==` 以及 `==` 和 `!=`.
  - 条件表达式的强制类型转换遵循以下规则：

    + **对象** 被计算为 **true**
    + **Undefined** 被计算为 **false**
    + **Null** 被计算为 **false**
    + **布尔值** 被计算为 **布尔的值**
    + **数字** 如果是 **+0, -0, or NaN** 被计算为 **false** , 否则为 **true**
    + **字符串** 如果是空字符串 `''` 则被计算为 **false**, 否则为 **true**

    ```javascript
    if ([0]) {
      // 返回true
      // 数组就是一个对象, 对象的返回值是true
    }
    ```

  - 使用快捷写法.

    ```javascript
    // 不推荐
    if (name !== '') {
      // ...stuff...
    }

    // 推荐
    if (name) {
      // ...stuff...
    }

    // 不推荐
    if (collection.length > 0) {
      // ...stuff...
    }

    // 推荐
    if (collection.length) {
      // ...stuff...
    }
    ```

  - 请参考更多资料 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108)

**[⬆ 回到顶部](#table-of-contents)**


## 代码块

  - 多行代码块用大括号.

    ```javascript
    // 不推荐
    if (test)
      return false;

    // 推荐
    if (test) return false;

    // 推荐
    if (test) {
      return false;
    }

    // 不推荐
    function() { return false; }

    // 推荐
    function() {
      return false;
    }
    ```

  - If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your
    `if` block's closing brace.
  - 当你代码中使用了 `if` 和 `else`语句，请把else放在if语句封口大括号的同一行.

    ```javascript
    // 不推荐
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // 推荐
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ 回到顶部](#table-of-contents)**


## 注释

  - 使用 `/** ... */` 写多行注释. I包括描述，指定类型以及所有的参数值和返回值.

    ```javascript
    // 不推荐
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // 推荐
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

  - 使用 `//` 写简单的单行注释. 单独占一行，写在需要评论的对象上面，注释和代码块前后空一行.

    ```javascript
    // 不推荐
    var active = true;  // 当前的TAB

    // 推荐
    // 当前的TAB
    var active = true;

    // 不推荐
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // 推荐
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing out a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME -- need to figure this out` or `TODO -- need to implement`.
  - 如果你有一个问题需要重新来看一下或如果你建议一个需要被实现的解决方法的话需要在你的注释前面加上 `FIXME` 或 `TODO` 帮助其他人迅速理解

  - 使用 `// FIXME:` 来注释问题.

    ```javascript
    function Calculator() {

      // FIXME: 这里不能使用全局变量
      total = 0;

      return this;
    }
    ```

  - 使用 `// TODO:` 注释问题的解决方法.

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

**[⬆ 回到顶部](#table-of-contents)**


## 空格

  - 设置TAB贱为2个空格.

    ```javascript
    // 不推荐
    function() {
    ∙∙∙∙var name;
    }

    // 不推荐
    function() {
    ∙var name;
    }

    // 推荐
    function() {
    ∙∙var name;
    }
    ```

  - 大括号前加个空格.

    ```javascript
    // 不推荐
    function test(){
      console.log('test');
    }

    // 推荐
    function test() {
      console.log('test');
    }

    // 不推荐
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // 推荐
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space before the argument list in function calls and declarations.

  - 
    ```javascript
    // 不推荐
    if(isJedi) {
      fight ();
    }

    // 推荐
    if (isJedi) {
      fight();
    }

    // 不推荐
    function fight () {
      console.log ('Swooosh!');
    }

    // 推荐
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - Set off operators with spaces.

    ```javascript
    // 不推荐
    var x=y+5;

    // 推荐
    var x = y + 5;
    ```

    - 语句的末尾添加回车换行符

    ```javascript
    // 不推荐
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // 不推荐
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // 推荐
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - 写JQuery长方法链，每个方法占一行，代码要缩进保持良好可读性和美观.

    ```javascript
    // 不推荐
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // 不推荐
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // 推荐
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // 不推荐
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // 推荐
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - 在代码块后面，表达式前面，单独空一行

    ```javascript
    // 不推荐
    if (foo) {
      return bar;
    }
    return baz;

    // 推荐
    if (foo) {
      return bar;
    }

    return baz;

    // 不推荐
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // 推荐
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```


**[⬆ 回到顶部](#table-of-contents)**

## 逗号

  - 不要将逗号放在最前

    ```javascript
    // 不推荐
    var story = [
        once
      , upon
      , aTime
    ];

    // 推荐
    var story = [
      once,
      upon,
      aTime
    ];

    // 不推荐
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // 推荐
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode. Also, in some implementations of ES3 would add length to an array if it had an additional trailing comma. This was clarified in ES5 ([source](http://es5.github.io/#D)):

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

  - 不要加多余的逗号，这可能会在IE下引起错误，同时如果多一个逗号某些ES3的实现会计算多数组的长度。

    ```javascript
    // 不推荐
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // 推荐
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 分号

  - 语句末尾一定要加分号

    ```javascript
    // 不推荐
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // 推荐
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // 推荐 (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ 回到顶部](#table-of-contents)**


## 类型转换

  - 在语句的开始执行类型转换.
  - 字符串:

    ```javascript
    //  => this.reviewScore = 9;

    // 不推荐
    var totalScore = this.reviewScore + '';

    // 推荐
    var totalScore = '' + this.reviewScore;

    // 不推荐
    var totalScore = '' + this.reviewScore + ' total score';

    // 推荐
    var totalScore = this.reviewScore + ' total score';
    ```

  - 对数字使用 `parseInt` 并且总是带上类型转换的进制数.
    ```javascript
    var inputValue = '4';

    // 不推荐
    var val = new Number(inputValue);

    // 不推荐
    var val = +inputValue;

    // 不推荐
    var val = inputValue >> 0;

    // 不推荐
    var val = parseInt(inputValue);

    // 推荐
    var val = Number(inputValue);

    // 推荐
    var val = parseInt(inputValue, 10);
    ```

  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // 推荐
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but Bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  - 布尔值:

    ```javascript
    var age = 0;

    // 不推荐
    var hasAge = new Boolean(age);

    // 推荐
    var hasAge = Boolean(age);

    // 推荐
    var hasAge = !!age;
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 命名约定

  - 避免无意义的单个字母，要求有语义化的命名.

    ```javascript
    // 不推荐
    function q() {
      // ...stuff...
    }

    // 推荐
    function query() {
      // ..stuff..
    }
    ```

  - 当命名对象、函数和实例时，使用驼峰命名法，且命名在上下文中有实际意义，增强代码可读性.

    ```javascript
    // 不推荐
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // 推荐
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - 当命名构造函数时，使用驼峰式命名，且首字母大写

    ```javascript
    // 不推荐
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // 推荐
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  -   - 私有属性命名，名称前加下划线 `_` .

    ```javascript
    // 不推荐
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // 推荐
    this._firstName = 'Panda';
    ```

  - 当`this`需要传值的时候，使用名称 `_this`，好有意义！
    ```javascript
    // 不推荐
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // 不推荐
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // 推荐
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - 给每个函数命名，避免匿名函数. 这样有助于在控制台调试和跟踪.

    ```javascript
    // 不推荐
    var log = function(msg) {
      console.log(msg);
    };

    // 推荐
    var log = function logText(msg) {
      console.log(msg);
    };
    ```

  - **Note:** IE8 and below exhibit some quirks with named function expressions.  See [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/) for more info.

  - 保持引用的文件名与接口名称一致。
    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    module.exports = CheckBox;

    // in some other file
    // 不推荐
    var CheckBox = require('./checkBox');

    // 不推荐
    var CheckBox = require('./check_box');

    // 推荐
    var CheckBox = require('./CheckBox');
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 存取器

  - 属性的存取器函数不是必需的.
  - If you do make accessor functions use getVal() and setVal('hello').
  - 如果你需要创建存取器请把他们命名为getVal() and setVal('hello').使用get和set单词，`get+什么`，`set+什么`方式命名.

    ```javascript
    // 不推荐
    dragon.age();

    // 推荐
    dragon.getAge();

    // 不推荐
    dragon.age(25);

    // 推荐
    dragon.setAge(25);
    ```

  - 如果属性是一个布尔值，请命名为isVal() 或 hasVal().使用is和has单词，`get+什么`，`set+什么`方式命名.
    ```javascript
    // 不推荐
    if (!dragon.age()) {
      return false;
    }

    // 推荐
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - 可以创建get()和set()函数，但是要保持一致.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 构造器

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // 不推荐
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // 推荐
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - 方法可以返回 `this` 帮助方法可链。

    ```javascript
    // 不推荐
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // 推荐
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - 可以写一个与JS语言自带的同名toString()方法，但是确保它工作正常并且不会有副作用，比如让它继承在某个对象下面。

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ 回到顶部](#table-of-contents)**


## 事件

  - When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:
  - - 当给事件附加数据时，传入一个哈希而不是原始值，这可以让后面的贡献者加入更多数据到事件数据里而不用找出并更新那个事件的事件处理器

    ```js
    // 不推荐
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    prefer:

    ```js
    // 推荐
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[⬆ 回到顶部](#table-of-contents)**


## 模块

  - 模块应该以 `!` 开始，这保证了如果一个有问题的模块忘记包含最后的分号在合并后不会出现错误
  - 这个文件应该以驼峰命名，并在同名文件夹下，同时导出的时候名字一致
  - 加入一个名为noConflict()的方法来设置导出的模块为之前的版本并返回它
  - 总是在模块顶部声明 `'use strict';`

    ```javascript
    // fancyInput/fancyInput.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

**[⬆ 回到顶部](#table-of-contents)**


## jQuery

  - JQuery对象使用`$`做为变量前缀
    ```javascript
    // 不推荐
    var sidebar = $('.sidebar');

    // 推荐
    var $sidebar = $('.sidebar');
    ```

    - 缓存Jquery查询的对象到一个变量中.

    ```javascript
    // 不推荐
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // 推荐
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.
  - 对DOM查询使用级联的 `$('.sidebar ul')` 或 `$('.sidebar ul')`，[jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - 对有作用域的jQuery对象查询使用 `find`

    ```javascript
    // 不推荐
    $('ul', '.sidebar').hide();

    // 不推荐
    $('.sidebar').find('ul').hide();

    // 推荐
    $('.sidebar ul').hide();

    // 推荐
    $('.sidebar > ul').hide();

    // 推荐
    $sidebar.find('ul').hide();
    ```

**[⬆ 回到顶部](#table-of-contents)**


## ECMAScript 5 Compatibility

  - Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/).

**[⬆ 回到顶部](#table-of-contents)**



## 性能

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[⬆ 回到顶部](#table-of-contents)**


## 资源


**Read This**

  - [Annotated ECMAScript 5.1](http://es5.github.com/)

**Tools**

  - Code Style Linters
    + [JSHint](http://www.jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Other Styleguides**

  - [Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://docs.jquery.com/JQuery_Core_Style_Guidelines)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/)
  - [JavaScript Standard Style](https://github.com/feross/standard)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](http://manning.com/vinegar/) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net) - Marijn Haverbeke
  - [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS) - Kyle Simpson

**博客**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](http://weblog.bocoup.com/)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](http://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://net.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](http://devchat.tv/js-jabber/)


**[⬆ 回到顶部](#table-of-contents)**

## 感谢

  这是使用本规范的一些组织的列表.他们给此规范提供了许多无私的帮助，所以把他们展示在这里.

  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **American Insitutes for Research**: [AIRAST/javascript](https://github.com/AIRAST/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **GeneralElectric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Nordic Venture Family**: [CodeDistillery/javascript](https://github.com/CodeDistillery/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **REI**: [reidev/js-style-guide](https://github.com/reidev/js-style-guide)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **Userify**: [userify/javascript](https://github.com/userify/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

## 翻译

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese(Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese(Simplified)**: [adamlu/javascript-style-guide](https://github.com/adamlu/javascript-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javacript-style-guide](https://github.com/mitsuruog/javacript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)

## JavaScript风格指南

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## 请与我们交流关于JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## 贡献者

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## 许可声明

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ 回到顶部](#table-of-contents)**
