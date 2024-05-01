## <mark style="background: #FF5582A6;">Asynchronous</mark>

나만의 정의 : 특정 조건을 만족하기 전에는 끝나지 않는 코드를 백그라운드에 넘기고 다음 코드와 동시에 작업하는 방식

```javascript
console.log('Start!');

fetch('https://www.google.com')
  .then((response) => response.text())
  .then((result) => { console.log(result); });

console.log('End'); 
```

위의 코드를 보면 다음의 과정이 존재한다.

1. console.log('Start')
2. fetch (request, register callback)
3. console.log('End')
4. response가 오면 then 메소드에 등록해뒀던 callback 실행

이처럼 특정 작업이 끝나기 전에 실행 흐름이 일단 다음 코드로 넘어가고 나중에 콜백이 실행되는 것을 Asynchronous 라고 한다.

Asynchronous가 없다면 위와 같은 상황에서 2번이 굉장히 길어졌을 꺼고 그 동안 코드 실행은 정지 되었을 것이다.

![[Pasted image 20240402064851.png]]

## <mark style="background: #FFB86CA6;">비동기 실행 함수들</mark>

## <mark style="background: #FFF3A3A6;">setTimeout</mark>

특정 함수의 실행을 원하는 시간만큼 뒤로 미루기 위해 사용하는 함수

```javascript
console.log('a');
setTimeout(() => { console.log('b'); }, 2000);
console.log('c');
```
![[Pasted image 20240402065349.png]]

callback이 실행되는 조건을 "설정된 시간만큼 시간이 경과했을 때" 이다.
이 역시 일종의 비동기적인 동작이라고 할 수 있다.

## <mark style="background: #FFF3A3A6;">setInterval</mark>

특정 콜백을 일정한 시간 간격으로 실행하도록 등록하는 함수

```javascript
console.log('a');
setInterval(() => { console.log('b'); }, 2000);
console.log('c');
```
![[Pasted image 20240402065521.png]]

setTimeout과 같이 조건은 "설정된 시간만큼 시간이 경과했을 때" 이다.

## <mark style="background: #FFF3A3A6;">addEventListener</mark>

특정 element의 특정 event에 콜백을 추가하는 함수

콜백이 일어나는 조건은 "설정된 event가 일어나야한다."

```javascript
...
btn.onclick = function (e) { // 해당 이벤트 객체가 파라미터 e로 넘어옵니다.
  console.log('Hello Codeit!');
};

// 또는 arrow function 형식으로 이렇게 나타낼 수도 있습니다. 
btn.onclick = (e) => {
  console.log('Hello Codeit!');
};
...
```

콜백이 당장 실행되는 것이 아니라 특정 조건에 따른다는 점에서 비동기적인 동작이라고 할 수 있다.

## <mark style="background: #FFF3A3A6;">fetch</mark>

```javascript
console.log('Start!');

fetch('https://www.google.com')
  .then((response) => response.text())
  .then((result) => { console.log(result); });

console.log('End'); 
```

이때 위의 세가지 함수들과 다르게 fetch는 Promise를 리턴해서 새로운 방식으로 비동기 동작을 수행한다.

## <mark style="background: #FFB86CA6;">Promise</mark>

## <mark style="background: #FFF3A3A6;">등장 배경</mark>

비동기적인 처리를 하는 법은 Promise 전에도 있었는데 이는 Callback Hell을 만들었다.

```javascript
setTimeout(function (name) {
  var coffeeList = name;
  console.log(coffeeList);
  setTimeout(function (name) {
    var coffeeList = name;
    console.log(coffeeList);
    setTimeout(function (name) {
      var coffeeList = name;
      console.log(coffeeList);
      setTimeout(function (name) {
        var coffeeList = name;
        console.log(coffeeList);
        setTimeout(function (name) {
          var coffeeList = name;
          console.log(coffeeList);
        });
      });
    });ㄴ
  });
});
```

## <mark style="background: #FFF3A3A6;">특징</mark>

작업에 관한 '상태 정보'

세가지 중 하나의 상태를 가진다.
1. pending
2. fulfilled
3. rejected

다음의 경우의 수가 있다.

1. fulfilled

	pending > fulfilled

이때 promise는 작업의 성공 결과를 가진다 이는 response 인자로 넘어간다.

## <mark style="background: #FFF3A3A6;">fulfilled시 콜백</mark>

```javascript
console.log('Start!');

fetch('https://www.google.com')
  .then((response) => response.text()) // fulfilled시 해당 내용에 대한 text에 대한 Promise를 리턴;
  .then((result) => { console.log(result); });

console.log('End'); 
```

2. rejected

	pending > rejected

이때 promise는 작업의 실패 결과를 가진다 이는 response 인자로 넘어간다.

## <mark style="background: #FFF3A3A6;">reject시 콜백</mark>

```javascript
fetch("www.naver.com")
  .then(
    (response) => response.text(),
    (error) => {
      console.log(error);
    }
  )
  .then((result) => {
    console.log(result);
  });
```

then 메소드의 두번째 parameter의 콜백은 reject시를 담당한다.

## <mark style="background: #FFF3A3A6;">Promise chaining</mark>

## <mark style="background: #BBFABBA6;">text, json 메소드도 Promise 객체를 리턴한다.</mark>

```javascript
// 1. text method
console.log('Start!');

fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.text())
  .then((result) => {
    const users = JSON.parse(result);
    // ...
  });

console.log('End'); 
```

text 메소드는 fulfilled 상태이면서 response body에 있는 내용을 string 타입으로 

변환한 값을 갖고 있는 promise 객체를 리턴한다. 이것이 JSON 데이터라면 

JSON.parse로 파싱 해줘야 한다.


```javascript
// 2. json method
console.log('Start!');

fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.json())
  .then((users) => {
    // ...
  });

console.log('End');
```

json 메소드는 fulfilled 상태이면 JSON데이터를 Deserialize한 자바스크립트 객체

를 갖고 있는 promise 객체를 리턴하고 rejected 상태이면 작업 실패 정보를 가진 

promise를 리턴한다.

## <mark style="background: #BBFABBA6;">Promise chaining이 필요한 이유</mark>

순차적으로 처리해야할 비동기 코드가 많아도 깔끔하게 처리할 수 있다.

```javascript
// 1. promise chaining 사용 전
fetch("www.naver.com")
  .then((response) => response.text())
  .then((result) => {
    const users = JSON.parse(result);
    const { id } = users[0];
    fetch(`www.naver.com/members/${id}`)
      .then((response) => response.text())
      .then((posts) => {
        console.log(posts);
      });
  });

// 2. promise chaining 사용 후
fetch("www.naver.com")
  .then((response) => response.text())
  .then((result) => {
    const users = JSON.parse(result);
    const { id } = users[0];
    return fetch(`www.naver.com/members/${id}`);
  })
  .then((response) => response.text())
  .then((posts) => {
    console.log(posts);
  });
```

또다른 callback hell을 해결할 수 있게 된다.

## <mark style="background: #FFF3A3A6;">.then method</mark>

```javascript
const successCallback = function () { };
const errorCallback = function () { };

fetch('https://jsonplaceholder.typicode.com/users') // Promise-A
  .then(successCallback, errorCallback); // Promise-B
```

Promise-A가 fulfilled면 successCallback이 실행되고 rejected면 errorCallback이 실행된다.

.then Method가 리턴하는 Promise-B는 내부 콜백함수가 리턴하는 값에 따라 상태, 

결과가 결정된다.

## <mark style="background: #BBFABBA6;">규칙</mark>

## <mark style="background: #ABF7F7A6;">실행된 콜백이 어떤 값을 리턴</mark>

## <mark style="background: #ADCCFFA6;">콜백이 Promise 객체를 리턴하는 경우</mark>

then method가 리턴한 promise 객체는 콜백에서 리턴한 promise 객체와 동일한 상태를 갖게 된다. (fulfilled, rejected) fulfilled일 경우 작업 성공 결과, reject일 경우 작업 실패 결과를 promise가 갖게 된다. 
![[Pasted image 20240402093231.png]]

ex)
```javascript
fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.json())
  .then((result) => { console.log(result) });
```

json() 메소드는 promise 객체를 리턴한다. 고로 이 리턴된 promise객체의 상태와 결과에 따라서 then 전체에서     

리턴되는 promise 객체의 상태와 결과가 정해진다는 것이다.

## <mark style="background: #ADCCFFA6;">콜백이 Promise 객체가 아닌 값을 리턴하는 경우</mark>

then method가 리턴한 promise 객체는 fulfilled 상태가 되고 작업 성공 결과를 담게 된다.
![[Pasted image 20240402093300.png]]

ex)
```javascript
// Internet Disconnected
fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.json(), (error) => 'Try again!')
  .then((result) => { console.log(result) });
```

인터넷이 끉겨서 errorcallback이 실행된 경우이다.

'Try again!' 이라는 string을 리턴하고 있다.

then method가 리턴하는 promise는 fulfilled 상태가 되고 값으로 'Try again!' 을 갖게 된다.

## <mark style="background: #ABF7F7A6;">실행된 콜백이 아무 값도 리턴하지 않음</mark>

```js
// Internet Disconnected
fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.json(), (error) => { alert('Try again!'); })
  .then((result) => { console.log(result) });
```

이때도 undefined를 리턴한 것이기 때문에 결과적으로 then method가 리턴한 promise 객체는 fulfilled 상태가 되고 작업 성공 결과를 담게 된다.

## <mark style="background: #ABF7F7A6;">실행된 콜백 내부에서 에러가 발생했을 때</mark>

```js
const promise = fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => { throw new Error('test'); });
```
![[Pasted image 20240402191922.png]]

then method가 리턴하는 promise는 rejected 상태가 되고 값으로 에러 객체를 갖게 된다.


## <mark style="background: #ABF7F7A6;">아무 콜백도 실행되지 않을때</mark>

```js
// Internet Disconnected
fetch('https://www.google.com') // Promise-1
  .then((response) => response.text()) // Promise-2
  .then((result) => { console.log(result) }, (error) => { alert(error) }); 
```

Promise-2에서 rejected상태를 받을 콜백이 없기에 Promise-1의 상태, 결과를 그대로 Promise-2이 갖게 하고 리턴시킨다.

## <mark style="background: #FFF3A3A6;">.catch</mark>

두번째 파라미터에 넣는 방식 말고 다른 방법으로 rejected를 처리하는 방법

```js

fetch('www.ndfasdfawefw.com')//없는 도메인
	.then((response)=>response.text())
	.then((result)=>{console.log(result)})
	.catch((error)=>{console.log(error)})
```

사실 then(undefined,(error)=>{console.log(error)})를 변형한 것이다.

다음 예시를 보자.

```js
// Internet Disconnected
fetch('https://jsonplaceholder.typicode.com/users') // Promise-A
  .then((response) => response.text()) // Promise-B
  .then(undefined, (error) => { console.log(error); }) // Promise-C
  .then((result) => { console.log(`Quiz: ${result}`); }); // Promise-D 
```
![[Pasted image 20240402193851.png]]

다음의 과정을 거친다.

1. Promise-A `rejected`
2. Promise-B 두번째 콜백이 없으니 Promise-A와 똑같이 만들고 리턴
3. Promise-C 두번째 콜백 실행, 리턴값은 없으니 `undefined` 리턴
4. Promise-D `undefined` 받고 `fulfilled` 첫번째 콜백 실행

## <mark style="background: #BBFABBA6;">catch 뒤에 then이 rejected되면?</mark>

catch를 가장 마지막에 쓰면 이유이다. 브라우저는 Promise가 rejected로 끝나

게 될 경우 에러로 인식한다. 이걸 잡으려면 마지막에 써야한다.

## <mark style="background: #BBFABBA6;">catch method를 여러 개 쓰는 경우</mark>

Promise chain 중간중간에 catch를 쓰는경우도 있는데 이때는 catch method가 중간에 에러가 발생해도 그걸 뒤로 

넘겨줄 수 있을 경우에만 해당한다.

```js
fetch('https://friendbook.com/my/newsfeeds')
  .then((response) => response.json()) // -- A
  .then((result) => { // -- B
    const feeds = result;
    // 피드 데이터 가공...
    return processedFeeds; 
  })
  .catch((error) => { // -- C
    // 미리 저장해둔 일반 뉴스를 보여주기  
    const storedGeneralNews = getStoredGeneralNews();
    return storedGeneralNews;
  })
  .then((result) => { /* 화면에 표시 */ }) // -- D
  .catch((error) => { /* 에러 로깅 */ }); // -- E
```

위 코드는 뉴스피드를 보여주는 코드이다.

## <mark style="background: #ABF7F7A6;">정상 작동</mark>

정상적으로 동작한다면 ABD가 실행되고 뉴스피드가 표시된다.

## <mark style="background: #ABF7F7A6;">에러 발생</mark>

	AB > C 작동 (미리 저장해둔 일반 뉴스를 보여준다, 대체 작업으로 정상적으로 끝나게 됨.) > D

에러가 발생했더라도 실패한 작업 대신에 대체 작업으로 정상적으로 끝낼 수 있는 경우라면 마지막에 둔 catch를 제

외하고 또 다른 catch를 중간에 놓으면 효율적이다.

## <mark style="background: #FFF3A3A6;">.finally</mark>

fulfilled이든  rejected이든 다 실행되는 메소드

보통 catch method 바로 뒤에 있다.

## <mark style="background: #FFF3A3A6;">Promise 객체</mark>

```js
const p = new Promise((resolve, reject)=>{
	//setTimeout(()=>{resolve('success')},2000);	
	setTimeout(()=>{reject(new Error('fail'))},2000);	
});
p.then((result)=>{console.log(result);})
p.catch((error)=>{console.log(error);})
```
## <mark style="background: #BBFABBA6;">executor</mark>
## <mark style="background: #ABF7F7A6;">resolve</mark>

fulfilled로 만들고 인자를 작업 성공 결과로 만드는 함수

## <mark style="background: #ABF7F7A6;">reject</mark>

reject로 만들고 인자를 작업 실패 결과로 만드는 함수

## <mark style="background: #FFF3A3A6;">Promisify</mark>

전통적인 형식의 비동기 실행 함수를 사용하는 코드를 Promise 기반으로 변환할 때 사용

ex1) setTimeout function
```js
function wait(text, milliseconds) {
  setTimeout(() => text, milliseconds);
}
```

## <mark style="background: #BBFABBA6;">ex) fetch 를 사용해서 Promise 기반으로 변환 </mark>

```js
// wait은 아무것도 리턴하지 않는 함수이다. > promise 객체를 만들어야 한다.
// function wait(text, milliseconds) {
//   setTimeout(() => text, milliseconds);
// }

function wait(text, milliseconds) {
  const p = new Promise((resolve, reject) => {
    setTimeout(() => { resolve(text); }, 2000);
  });
  return p;
}

fetch('https://jsonplaceholder.typicode.com/users')
  .then((response) => response.text())
  .then((result) => wait(`${result} by Codeit`, 2000)) // 2초 후에 리스폰스의 내용 뒤에 'by Codeit' 추가하고 리턴
  .then((result) => { console.log(result); });

```
![[Pasted image 20240403085913.png]]

잘 동작하는 것을 알 수 있다.

이렇게 함수를 Promise 기반으로 변환시키는 걸 `promisify`라고 한다.

## <mark style="background: #BBFABBA6;">ex) callback hell 해결( Node.js의 readFile 비동기 실행 메소드 )</mark>

```js
fs.readFile('file1.txt', 'utf8', (error1, data1) => {
  if (error1) {
    console.log(error1);
  } else {
    console.log(data1);
    fs.readFile('file2.txt', 'utf8', (error2, data2) => {
      if (error2) {
        console.log(error2);
      } else {
        console.log(data2);
        fs.readFile('file3.txt', 'utf8', (error3, data3) => {
          if (error3) {
            console.log(error3);
          } else {
            console.log(data3);
          }
        });
      }
    });
  }
});

```

promisify를 적용하지 않으면 Callback Hell이 발생한다.

```js
function promisify(filename) {
  const p = new Promise((resolve, reject) => {
    fs.readFile(filename, "utf8", (error, data) => {
      if (error) {
        reject(error);
      }
      if (data) {
        resolve(data);
      }
    });
  });
  return p;
}

promisify('file1.txt')
	.then((resolve)=>{console.log(resolve); return promisify('file2.txt');})
	.then((resolve)=>{console.log(resolve); return promisify('file3.txt');})
	.then((resolve)=>{console.log(resolve); return promisify('file4.txt');})
```


## <mark style="background: #BBFABBA6;">Promisify를 하면 안되는 경우</mark>

Promise 객체는 한번 pending 상태에서 fulfilled 또는 rejected 상태가 되고 나면 그 뒤로는 그 상태와 결과가 변하지 않는다.

```js
const box = document.getElementById('test');
let count = 0;

function addEventListener_promisified(obj, eventName) { // 이런 Promisify는 하지 마세요
  const p = new Promise((resolve, reject) => {
    obj.addEventListener(eventName, () => { // addEventListener 메소드
      count += 1;
      resolve(count);
    });
  });
  return p;
}

addEventListener_promisified(box, 'click')
	.then((eventCount) => { console.log(eventCount); });
```

이때 실행하면 한번만 실행되고 그 다음 count 값들은 출력되지 않는다.

다음의 과정이 흐른다.
1. box에 click event 발생
2. Promise 생성 
3. Promise 내부에서 resolve로 fulfilled상태 그리고 작업 성공 결과 설정
4. console.log(count)
5. 그 다음에 이벤트가 발생되도 Promise객체가 한번 결정한건 바뀌지 않는다.

고로 Promisify는 콜백이 한번만 실행되는 함수에만 사용해야 한다.

## <mark style="background: #FFF3A3A6;">이미 상태가 결정된 Promise 객체</mark>

처음부터 fulfilled, rejected인 Promise 객체를 만들 수 있다.

기존의 형식(`new Promise`)과 똑같이 then이나 catch 이런거 다 똑같이 동작하는데 위의 특징만 갖고 있다고 보면 됨.

## <mark style="background: #BBFABBA6;">fulfilled 객체 만들기</mark>

```js
const p = Promise.resolve('success');
p.then((result) => { console.log(result); }, (error) => { console.log(error); });
```

## <mark style="background: #BBFABBA6;">rejected 객체 만들기</mark>

```js
const p = Promise.reject(new Error('fail'));
p.then((result) => { console.log(result); }, (error) => { console.log(error); });
```

## <mark style="background: #ABF7F7A6;">rejected 객체 활용</mark>

```js
function doSomething(a, b) {
    //~~
  if (problem) {
    throw new Error('Failed due to..'));
  } else {
    return fetch('https://~');
  }
}

//여기서 Error가 아니라 Error를 담은 rejected Promise 객체를 만들어서 처리할 수 있다.
function doSomething(a, b) {
  // ~~
  if (problem) {
    return Promise.reject(new Error('Failed due to..'));
  } else {
    return fetch('https://~');
  }
}
```

## <mark style="background: #FFF3A3A6;">Promise 객체의 작업 성공 결과 또는 작업 실패 정보</mark>

Promise가 fulfilled, rejected 상태이기만 하면 언제 then method로 콜백을 붙히든지간에 작업 결과를 사용할 수 있다.

```js
const p = new Promise((resolve, reject) => {
  setTimeout(() => { resolve('success'); }, 2000); // 2초 후에 fulfilled 상태가 됨
});

// Promise 객체가 pending 상태일 때 콜백 등록
p.then((result) => { console.log(result); }); 

// Promise 객체가 fulfilled 상태가 되고 나서 콜백 등록
setTimeout(() => { p.then((result) => { console.log(result); }); }, 5000); 
```

## <mark style="background: #FFF3A3A6;">여러 Promise 객체를 다루는 방법</mark>

## <mark style="background: #BBFABBA6;">all method</mark>

```js
// 1번 직원 정보
const p1 = fetch('https://learn.codeit.kr/api/members/1').then((res) => res.json());
// 2번 직원 정보
const p2 = fetch('https://learn.codeit.kr/api/members/2').then((res) => res.json());
// 3번 직원 정보
const p3 = fetch('https://learn.codeit.kr/api/members/3').then((res) => res.json());

Promise
  .all([p1, p2, p3])
  .then((results) => {
    console.log(results); // Array : [1번 직원 정보, 2번 직원 정보, 3번 직원 정보]
  })
  . catch((error)=>{
	console.log(error);
  })
```

배열 안의 모든 promise 객체가 fulfilled가 되면 all 메소드가 fulfilled promise 리턴
하나라도 rejected이면 all 메소드가  rejected promise 리턴![[Pasted image 20240403125806.png]]
## <mark style="background: #BBFABBA6;">race method</mark>

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Success'), 1000);
});
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error('fail')), 2000);
});
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error('fail2')), 4000);
});

Promise
  .race([p1, p2, p3])
  .then((result) => {
    console.log(result); // hello 출력
  })
  .catch((value) => {
    console.log(value);
  });
```

가장 먼저 fulfilled나 reject된 promise가 있으면 그걸 race가 받아서 상태, 결과 리턴
![[Pasted image 20240403125754.png]]

## <mark style="background: #BBFABBA6;">allSettled method</mark>

resolved와 rejected를 묶어서 settled 상태라고 한다.

배열로 받아서 pending만 아니면 fulfilled된 후에 각 요소의 실패, 성공을 다 정리해주는 메소드

```js
[
   {status: "fulfilled", value: 1},
   {status: "fulfilled", value: 2},
   {status: "fulfilled", value: 3},
   {status: "rejected",  reason: Error: an error}
]
```

## <mark style="background: #BBFABBA6;">any method</mark>

먼저 fulllfilled된 Promise의 상태와 결과가 리턴될 promise에 반영

모든 Promise가 rejected될 경우 AggregateError를 작업 실패 정보로 갖고 rejected

하나라도 fulfilled이면 fulfilled가 리턴되는 method

## <mark style="background: #FFF3A3A6;">axios</mark>

```js
axios
  .get('https://jsonplaceholder.typicode.com/users')
  .then((response) => {
    console.log(response);
  })
  .catch((error) => {
    console.log(error);
  });
```

fetch와 다른 점들
1. 모든 request, response에 대한 공통 설정, 전처리 함수 삽입
2. serialization, deserialization 자동 수행
3. request timeout(request 취소 설정)
4. 업로드 시 진행 상태 정보를 얻을 수 있음.

fetch와 같이 Promise 객체를 리턴하기 때문에 위의 코드를 보면 유사한 점이 많다.

패키지를 별도로 다운로드 해야된다.
[axios](https://github.com/axios/axios)

## <mark style="background: #FFB86CA6;">async/await</mark>

Promise객체를 더 쉽게 다룰 수 있는 문법

```js
// fetch('www.naver.com')
//    .then((response)=>response.text())
//    .then((result)=>{console.log(result)})

/* fetch('https://www.google.com') .then((response) => response.text()) .then((result) => { console.log(result); }); */
// fetch로 만든 코드
function fetchAndPrint() {s
  console.log(2);
  fetch("https://jsonplaeholder.typicode.com/users")
    .then((response) => {
      console.log(7);
      return response.text();
    })
    .then((result) => {
      console.log(result);
    });
}
console.log(1);
  
// async await로 만든 코드
async function fetchAndPrint() {
  console.log(2);
  const response = await fetch("www.naver.com");
  const result = await response.text();
  console.log(7);
}
console.log(1);
fetchAndPrint();
console.log(3)
console.log(4);
console.log(5);
console.log(6);
/*
1
2
3
4
5
6
7
*/
```

async는 비동기적으로 실행된다는 것을 의미하고 await은 뒤에 나오는 Promise가 fulfilled가 될때까지 기다린다는 것이다.

결과적으로 await 뒤에는 Promise 객체를 리턴한다고 보면 된다.
(await은 async function 안에서만 사용할 수 있다.)

await을 만나면 백그라운드로 보내고 해당 함수의 바깥으로 흐름이 잠시 이동함

위의 코드를 예시로 들면 fetch.then이 fetchAndPrint 함수 호출 밑에 있는 console.log(3)이라고 생각하면 된다.

## <mark style="background: #FFF3A3A6;">try catch, finally</mark>

rejected 상태를 다루는 방법

.finally method처럼 사용할 수 있는 finally도 있다.

```js
// async await로 만든 코드
try{
async function fetchAndPrint() {
  console.log(2);
  const response = await fetch("www.naver.com");
  const result = await response.text();
  console.log(7);
}
console.log(1);
fetchAndPrint();
console.log(3)
console.log(4);
console.log(5);
console.log(6);
/*
1
2
3
4
5
6
7
*/
} catch (error){
	console.log(error);
} finally{
	console.log('exit');
}
```

## <mark style="background: #FFF3A3A6;">async 함수와 Promise 객체와의 관계</mark>

**async 함수는 항상 Promise 객체를 리턴한다.**

## <mark style="background: #BBFABBA6;">규칙</mark>

## <mark style="background: #ABF7F7A6;">어떤 값을 리턴하는 경우</mark>

## <mark style="background: #ADCCFFA6;">Promise 리턴</mark>

해당 Promise와 동일한 상태와 작업 성공 결과(작업 실패 정보)를 가진 Promise 객체를 리턴

```js
async function fetchAndPrint() {
  return new Promise((resolve, reject)=> {
    setTimeout(() => { resolve('abc'); }, 4000);
  });
}

fetchAndPrint(); // 처음엔 pending 4초 후에 fulfilled
```
![[Pasted image 20240403201026.png]]

## <mark style="background: #ADCCFFA6;">Promise 이외의 값 리턴</mark>

fulfilled이면서 리턴된 값을 작업 성공 결과로 가진 Promise 객체 리턴

```js
async function fetchAndPrint() {
  return 3;
}

fetchAndPrint();
```
![[Pasted image 20240403201228.png]]

```js
async function fetchAndPrint() {
  const member = {
    name: 'Jerry',
    email: 'jerry@codeitmall.kr',
    department: 'sales',
  };
  return member;
}
fetchAndPrint();
```
![[Pasted image 20240403201338.png]]

## <mark style="background: #ABF7F7A6;">아무 값도 리턴하지 않는 경우</mark>

fulfilled이고 `undefined`를 작업 성공 결과로 가진 Promise 객체 리턴

```js
async function fetchAndPrint() {
  console.log('Hello Programming!');
}

fetchAndPrint();
```
![[Pasted image 20240403201402.png]]
## <mark style="background: #ABF7F7A6;">내부에서 에러가 발생했을 때</mark>

rejected이고 PromiseResult로 해당 Error를 가진 Promise 객체 리턴

```js
async function fetchAndPrint() {
  throw new Error('Fail');
}

fetchAndPrint();
```
![[Pasted image 20240403201446.png]]

## <mark style="background: #ABF7F7A6;">이로써 할 수 있는 것</mark>

Promise를 무조건 리턴하기 때문에 async 안에서 async를 쓸 수 있게 된다.

## <mark style="background: #FFF3A3A6;">nested async function</mark>

```js
const applyPrivacyRule = async function (users) {
  const resultWithRuleApplied = users.map((user) => {
    const keys = Object.keys(user);
    const userWithoutPrivateInfo = {};
    keys.forEach((key) => {
      if (key !== "address" && key !== "phone") {
        userWithoutPrivateInfo[key] = user[key];
      }
    });
    return userWithoutPrivateInfo;
  });
  
  const p = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(resultWithRuleApplied), 2000;
    });
  });
  return p;
};
  
getUsers = async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/users");
    const result = await response.json();
    const users = JSON.parse(result);
    const preprocessedUser = await applyPrivacyRule(users);
    return preprocessedUser;
  } catch (error) {
    console.log(error);
  } finally {
    console.log("exit");
  }
};

getUsers().then((result) => {
  console.log(result);
});
```

## <mark style="background: #FFF3A3A6;">async의 위치</mark>

```js
// 1) Function Declaration
async function example1(a, b) {
  return a + b;
}

// 2-1) Function Expression(Named)
const example2_1= async function add(a, b) {
  return a + b;
};

// 2-2) Function Expression(Anonymous)
const example2_2 = async function(a, b) {
  return a + b;
};

// 3-1) Arrow Function
const example3_1 = async (a, b) => {
  return a + b;
};

// 3-2) Arrow Function(shortened)
const example3_2 = async (a, b) => a + b;

// IIFE 방식
(async function print(sentence) {
  console.log(sentence);
  return sentence;
}('I love JavaScript!'));

(async function (a, b) {
  return a + b;
}(1, 2));

(async (a, b) => {
  return a + b; 
})(1, 2);

(async (a, b) => a + b)(1, 2);
```

## <mark style="background: #FFF3A3A6;">async function 의 성능 문제</mark>

```js
async function getResponses(urls) {
  for(const url of urls){
    const response = await fetch(url);
    console.log(await response.text());
  }
}
```

위 코드는 aynchronous function을 사용하지만 내부적으로 for문을 돌리며 두줄의 await들이 끝나지 않으면 다음 루프로 넘어가지 않는 안 좋은 성능을 가지고 있다.

```js
async function fetchUrls(urls){
  for(const url of urls){
    (async () => { // 추가된 부분!
      const response = await fetch(url);
      console.log(await response.text());
    })(); // 추가된 부분!
  }
}
```

위 코드처럼 IIFE를 사용하여 비동기성을 더 강화해야한다. (단, 순서는 랜덤으로 되게 된다.)

내생각 : 호출 스택 생각해보자. 
1. for문 위에 두줄의 await가 아닌 `function 호출` 이라는 컨텍스트가 올라가고 
2. 모든 for문에서 순식간에 function들의 호출이 끝나고 
3. 각 `function 호출`은 실행될때 백그라운드에 해당 내용을 넘기고 
4. 백그라운드에서는 해당 await이 끝나는대로 테스크 큐로 결과를 넘기고 
5. 테스크 큐는 for문이 끝나고 전역 컨텍스트가 끝나면 호출스택으로 결과들을 넘긴다. 
6. 백그라운드에서 실행완료되는 것은 랜덤이기 때문에 순서가 랜덤

## <mark style="background: #FFB86CA6;">두 가지 종류의 콜백</mark>

## <mark style="background: #FFF3A3A6;">콜백</mark>

어떤 함수의 파라미터로 전달되는 모든 함수를 의미

## <mark style="background: #FFF3A3A6;">Synchronous callback</mark>

## <mark style="background: #BBFABBA6;">ex) filter method</mark>

```js
const arr = [1, 2, 3, 4, 5, 6];

const newArr = arr.filter(()=> num % 2); 
});

console.log(newArr); // [1, 3, 5]
```

여기서 `()=>num%2` 는 Synchronous callback이다. (순서대로 실행된다.)

## <mark style="background: #FFF3A3A6;">Asynchronous callback</mark>