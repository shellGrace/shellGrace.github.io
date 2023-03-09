---
date: 2020-08-12
title: apply、call和 bind
categories:
  - javascript
featured_image: images/js.jpg
---
## bind
bind() 方法创建一个新的函数，在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

## apply
```javascript
var person = {
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
}
var person1 = {
    firstName: "Bill",
    lastName: "Gates",
}
person.fullName.apply(person1);  // 将返回 "Bill Gates"
```
person.fullName 中的 this 会去 person1中寻找

## call
```javascript
var person = {
    fullName: function() {
        return this.firstName + " " + this.lastName;
    }
}
var person1 = {
    firstName:"Bill",
    lastName: "Gates",
}
var person2 = {
    firstName:"Steve",
    lastName: "Jobs",
}
person.fullName.call(person1);  // 将返回 "Bill Gates"
person.fullName.call(person2);  // 将返回 "Steve Jobs"
```
### 带参数的 call() 方法
```
var person = {
  fullName: function(city, country) {
    return this.firstName + " " + this.lastName + "," + city + "," + country;
  }
}
var person1 = {
  firstName:"Bill",
  lastName: "Gates"
}
person.fullName.call(person1, "Seattle", "USA");
```

### 带参数的 apply() 方法
```
var person = {
  fullName: function(city, country) {
    return this.firstName + " " + this.lastName + "," + city + "," + country;
  }
}
var person1 = {
  firstName:"Bill",
  lastName: "Gates"
}
person.fullName.apply(person1, ["Oslo", "Norway"]);
```

一个特例
```javascript
Math.max(1,2,3);  // 会返回 3
Math.max.apply(null, [1,2,3]); // 也会返回 3
```
第一个参数（null）无关紧要。在本例中未使用它。

### call() 和 apply() 之间的区别
* call() 方法分别接受参数。
* apply() 方法接受数组形式的参数。

## 关于this
![](/i/2b605e43-cfcd-43fe-8cd7-78bea91f01c3.jpg)
this 永远指向最后调用它的那个对象，即：谁调用函数，this就指向谁

如何改变 this 的指向
* 使用 ES6 的箭头函数
* 在函数内部使用 _this = this
* 使用 apply、call、bind
* new 实例化一个对象

```javascript
var name = "windowsName";
    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            setTimeout(  function () {
                this.func1()
            },100);
        }

    };

    a.func2()     // this.func1 is not a function
```
在不使用箭头函数的情况下，是会报错的，因为最后调用 setTimeout 的对象是 window，但是在 window 中并没有 func1 函数。

ES6 的箭头函数可以避免 ES5 中使用 this 的坑.
箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined”。


