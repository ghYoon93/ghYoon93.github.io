---
title: 데이터 타입, let vs var, hoisting
categories: [JavaScript]
tags: [variable, data type, hosting]
---

자바스크립트에서 사용하는 데이터 타입과 let과 var의 차이점, hoisting을 알아보자

## Use strict

ES 5에 추가된 개념,  JavaScript 의 제한된 버전을 선택하여 암묵적인 "느슨한 모드(sloppy mode)" 를 해제하기 위한 방법

```javascript
//1. Use strict
// added in ES 5
// use this for Vanila Javascript
'use strict';
```



## Variable

#### let

ES 6에 추가된 변수, block scope를 갖고 있으며 초기화 후 다른 값을 저장할 수 있다.

```javascript
// 2. Variable, rw(read/write)
// let (added in ES 6)
let globalName = 'global name';
{
  let name = "ygh";
  console.log(name);
  name = "yghee";
  console.log(name);
  console.log(globalName);
}
console.log(name);
console.log(globalName);
```



#### var

ES 6 이전에 쓰던 변수, block scope가 없어 어느 블록에 선언하고 저장해도 사용할 수 있다. hoisting을 할 수 있는 특징이 있다.

**hoisting**은 변수나 함수 선언을 상단으로 끌어올리는 것으로 선언 전에 이를 호출해도 정상적으로 작동한다.

```javascript

// var (don't ever use this!)
// var hoisting (move declaration from bottom to top)
// has no block scope
{
  age = 4;
  var age;
}
console.log(age); // 4
```



#### Constant

ES 6에서 let과 함께 추가된 변수 타입으로 초기화 후 다른 데이터 값을 저장할 수 있다. 값의 변화가 없을 때 사용을 최대한 권장하고 있으며 그 이유는 다음과 같다.

* security
* thread safety
* reduce human mistakes

```javascript
// 3. Constant, r(read only)
// use const whenever possible.
// only use let if variable needs to change.
const daysInWeek = 7;
const maxNumber = 5;

// Note!
// Immutable data types: primitive types, frozen objects (i.e. object.freeze())
// Mutable data types: all objects by default are mutable in JS
// favor immutable data type  always for a few reasons:
// - security
// - thread safety
// - reduce human mistakes
```



## Variable types

#### primitive (single item)

**number**

```javascript
const count = 17; // integer
const size = 17.1; // decimal number
console.log(`value: ${count}, type: ${typeof count}`);
console.log(`value: ${size}, type: ${typeof size}`);

// number - special numeric values: infinity, -infinity, NaN
const infinity = 1 / 0;
const negativeInfinity= -1 / 0;
const nAn = 'not a number' / 2;
console.log(infinity);
console.log(negativeInfinity);
console.log(nAn);

// bigInt (fairly new, don't use it yet)
const bigInt = 123456789012345678901234567890123456789012345678901234567890n;
console.log(`value: ${bigInt}, type: ${typeof bigInt}`);
Number.MAX_SAFE_INTEGER;
```



**string**

```javascript
// string
const char = 'c';
const brendan = 'brendan';
const greeting = 'hello ' + brendan;
console.log(`value: ${greeting}. type: ${typeof greeting}`);
const helloBrendan = `hi ${brendan}!`; // template literals (string)
console.log(`value: ${helloBrendan}. type: ${typeof helloBrendan}`);
```



**boolean**

```javascript
// boolean
// false: 0, null, undefined, NaN, ''
// true:any other value
const canRead = true;
const test= 3  < 1; // flase
console.log(`value: ${canRead}, type: ${typeof canRead}`);
console.log(`value: ${test}, type: ${typeof test}`);
```



**null**

```javascript
// null
let noting = null;
console.log(`value: ${noting}, type: ${typeof nothing}`);
```



**undefined**

```javascript
// undefined
let x;
console.log(`value: ${x}, type: ${typeof x}`);
```



**symbol**

```javascript
// symbol, create unique identifiers for objects
const symbol1 = Symbol('id');
const symbol2 = Symbol('id');
console.log(symbol1 === symbol2);
const gSymbol1 = Symbol.for('id');
const gSymbol2 = Symbol.for('id');
console.log(gSymbol1 === gSymbol2);
console.log(`value: ${symbol1.description}, type: ${typeof symbol1}`);
```



#### object (box container)

```javascript
// object, real-life object, data structure
const yghee = {name: 'yghee',  age:  20};
console.log(yghee.age);
yghee.age = 13;
console.log(yghee.age);
```



#### function (first-class function)



## Dynamic typing

```javascript
// 5. Dynamic typing: dynamicallytyped language
let text = 'hello';
console.log(text.charAt(0)); // h
console.log(`value: ${text}, type: ${typeof text}`);
text  = 1;
console.log(`value: ${text}, type: ${typeof text}`);
text = '7' + 5;
console.log(`value: ${text}, type: ${typeof text}`);
text = '8' / '2';
console.log(`value: ${text}, type: ${typeof text}`);
console.log(text.charAt(0));

```

