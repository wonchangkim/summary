### setInterval

```js
const intervalId = setInterval( () => {
  console.log('3초마다 출력됨');
}, 3000);

setTimeout( () => {
  clearInterval(intervalId);
}, 10000);

//3초마다 출력됨
//3초마다 출력됨
//3초마다 출력됨
//세번 출력 후 끝

```

delay(cb, 3000);
```js
function delay(cb, time) {
  return function () {
    setTimeout(cb, time);
  }
}

const f = delay(() => console.log('hello'),3000);
f();
```
debounce(cb, 3000)
문서저장, 일정시간이 지난 후 실행
throttle(cb, 3000)
뱀게임
```js
function delay(cb, time) {
  return function() {
    setTimeout(cb, time);
  }
}

function debounce(cb, time) {
  let timeoutId;
  return function() {
    if (timeoutId) {
      clearTimeout(timeoutId);
    }
    timeoutId = setTimeout(() => {
      cb();
      timeoutId = null;
    }, time)
  }
} 
/*
debounce함수
콜백함수(cb)가 반복될 땐 실행되지 않다가, 콜백함수가 모두 끝나면 실행되는 함수
반환함수에서 예약된 타이머를 setTimeout사용하여 취소
*/

function throttle(cb, time) {
  let throttled = false;
  let timeoutId;
  return function() {
    if (!throttled) {
      cb();
    }
    throttled = true;
    if (!timeoutId) {
      timeoutId = setTimeout(() => {
        throttled = false;
        timeoutId = null;
      }, time);
    }
  }
}

//일정시간이 지나기 전까지 콜백함수를 막아주는 함수
```
---
### Promise
- Promise객체에 값이 담기는 순간 콜백함수 동작
- 콜백함수 등록은 .then()메소드를 사용
- Promise객체에 값이 담기기 이전에 콜백함수를 등록할 수 있음
```js
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('2초가 지났습니다.');
    resolve('hello');
  }, 2000);
});

//메소드 호출의 결과값이 promise
p.then(msg => {
  return msg + ' world';
}).then(msg => {
  console.log(msg);
});

```
.then().then() 

Task Queue와 호출스택 사이에 중간 작업점이 존재
작업큐와 마이크로 작업큐가 있음
프로미스는 별도의 큐가 존재함(마이크로작업큐)
마이크로작업큐가 작업큐보다 먼저 실행되므로 동기식처럼 보이나
엄밀히 따지면 각각 다른 작업큐를 사용하므로 비동기방식
then은 새로운 프로미스를 반환


```js
const p = Promise.resolve(2);

p.then( () => {
  return Promise.resolve(2);
}).then( value => { //Promise.resolve(2)에 담긴 값 2가 value에 담김
  console.log(value);
});
```