---
title: JavaScript async와 await 그리고 유용한 Promise APIs
categories: [JavaScript]
tags: [async, await, APIs]
---

비동기의 꽃 async & await와 유용한 Promise API들을 알아보자.



## Async & await

Async와 await는 기존에 존재하는 `promise`를 좀 더 간편하게 쓸 수 있게 해주는 API

비동기적으로 실행되는 `Promise`가 동기적으로 실행되는 것처럼 보이게 해준다.

#### Async

`async function functionName(parameter) {}`

`aysnc`는 함수를 선언하는 방식으로 함수를 비동기 함수로 만들어준다.

반환값은 `promise`의 `resolve`이며 `throw`로 `reject`를 다룬다.



**기존 방식**

```javascript
async function fetchUser() {
  // do network request in 10 secs...
  return new Promise((resolve, reject) => {
    if(noError) {
      resolve('yghee');
    } else {
        reject(new Error('error...'));
    }
  }
}

const user = fetchUser();
user.then(console.log); // {<fulfilled>:"yghee"}
console.log(user);
```



**async 선언 방식**

```javascript
// 1. async
async function fetchUser() {
  // do network request in 10 secs...
    if(noError) {
        return 'yghee';
    }else  {
        throw new Error('errors');
    }
}

const user = fetchUser();
user.then(console.log); // {<fulfilled>:"yghee"}
console.log(user);
```



#### Await

`await`는 `async` 함수의 실행을 일시 중지하고 전달 된 `Promise`의 해결을 기다린 다음 `async` 함수의 실행을 다시 시작하고 완료 후 값을 반환한다.

`await`는 `async`가 선언된 함수에서만 사용 가능하다. 



**기존 방식**

```javascript
// 2. await
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function getApple() {
  delay(2000);
  return '🍎';
}

async function getBanana()  {
  await delay(1000);
  return '🍌';
}
```



**await 방식**

```javascript
// 2. await
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function getApple() {
  await delay(2000);
  return '🍎';
}

async function getBanana()  {
  await delay(1000);
  return '🍌';
}

async function pickFruits() {
  const applePromise = getApple();
  const bananaPromise = getBanana();
  const apple = await applePromise;
  const banana = await bananaPromise;
  return `${apple} + ${banana}`;
}

pickFruits().then(console.log); // 3초 후 🍎 + 🍌가 반환
```



#### 유용한 Promise APIs

```javascript


// 3. useful Promise APIs
function pickAllFruits() {
  return Promise.all([getApple(), getBanana()])
    .then(fruits => fruits.join(' + '));
}

pickAllFruits().then(console.log);

function pickOnlyOne() {
  return Promise.race([getApple(), getBanana()]);
}

pickOnlyOne().then(console.log);
```

