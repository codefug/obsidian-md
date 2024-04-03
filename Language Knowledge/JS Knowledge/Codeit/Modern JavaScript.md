## <mark style="background: #FF5582A6;">ECMAScript</mark>

ES1 : 1997 > ES6 : 2015 부터 1년주기로 업데이트 되는 중 (이제 ECMASscript2015 형식으로 바뀜)

Modern JavaScript : 현시점에 사용하기 적합한 범위 내에서 최신 버전의 표준을 준수하는 자바스크립트

JavaScript !== ECMAScript

JS에는 DOM같은 WebIDL에서 표준화된 기술도 존재한다.

## <mark style="background: #FF5582A6;">Data Type && Operator</mark>

## <mark style="background: #FFB86CA6;">Data Type</mark>

자바스크립트는 데이터 타입에서 유연하게 변한다.

## <mark style="background: #FFF3A3A6;">데이터 종류</mark>

| Primitive Type | Reference Type |
| -------------- |:-------------- |
| Number         | Object         |
| String         |                |
| Boolean        |                |
| Null           |                |
| Undefined      |                |
| Symbol         |                |
| BigInt         |                |
## <mark style="background: #BBFABBA6;">Symbol</mark>

Symbol() 이라는 메소드로 만드는 기본형 데이터

어떤 값과 비교해도 true가 될 수 없는 고유한 변수가 된다.
(같은 Symbol이여도 무조건 false를 반환한다.)
```javascript
const user1 = Symbol('this is a user');
const user2 = Symbol('this is a user');

user1 === 'this is user'; // false
user1 === 'user'; // false
user1 === 'Symbol'; // false
user1 === true; // false
user1 === false; // false
user1 === 123; // false
user1 === 0; // false
user1 === null; // false
user1 === undefined; // false
user1 === user2; //false
```

## <mark style="background: #BBFABBA6;">BigInt</mark>

정수 끝에 n을 붙히거나 BigInt(<b>'number'</b>)로 만드는 아주 큰 정수를 표현하는 데이터 타입
(암호 관련 작업이나 계산기 관련 작업에서 필요하다.)

JS는 IEEE 754에서 정한 부동 소수점 형식 숫자 체계를 사용하는데 이로 인해 2<sup>53</sup>-1(약 9000조) 정도의 정수 표현의 한계가 존재한다.

```javascript
console.log(9007199254740991 + 1 === 9007199254740991 + 2); // true
console.log(9007199254740991 + 2); /// 9007199254740992
console.log(9007199254740993); /// 9007199254740992
```

## <mark style="background: #ABF7F7A6;">BigInt를 이용해서 이 한계를 부순다.</mark>

```javascript
console.log(9007199254740993n); // 9007199254740993n
console.log(BigInt('9007199254740993')); // 9007199254740993n
```

BigInt를 사용할 경우 안에 그냥 정수를 사용하면 자동으로 정수가 한계점으로 줄어들기 때문에 문자열 형태로 넣어야 한다.

## <mark style="background: #ABF7F7A6;">BigInt를 이용한 연산</mark>

소수가 나오는 연산의 경우 floor 처리된다.

```javascript
10n / 6n; // 1n
5n / 2n; // 2n
```

다른 데이터 타입과 비교하면 <b>TypeError</b>
Number()로 명시적 타입 변환해줘야 한다.

```javascript
3n * 2; // TypeError
3n * 2n; // 6n
Number(3n) * 2; // 6
```

## <mark style="background: #BBFABBA6;">Boolean</mark>

## <mark style="background: #ABF7F7A6;">falsy</mark>

boolean이 필요할 때 자동으로 false로 형변환되는 값
> `null`, `undefined`, `false`, `NaN`, `0`,`-0`,`0n`,`""`,`document.all` 이렇게 9개만 falsy임

```javascript
const flowers = ['장미', '수국', '백합', '튤립', '진달래'];

for (let i = 4; i; i = i - 2) { 

// i==0일때 조건문 i를 만나서 for문을 나와버린다.

console.log(flowers[i]);
}

if (flowers.length) {
  console.log(flowers[3]);
}

if (Number('codeit')) {
  console.log(flowers[1]);
}

```

## <mark style="background: #ABF7F7A6;">truthy</mark>

boolean이 필요할 때 자동으로 true로 형변환되는 값
falsy를 제외한 모든 값

## <mark style="background: #FFB86CA6;">연산자</mark>

## <mark style="background: #FFF3A3A6;">typeof 연산자</mark>

데이터 타입을 확인할 수 있는 연산자 함수처럼 사용할 수도 있다.

```javascript
typeof 'Codeit'; // string
typeof Symbol(); // symbol
typeof {}; // object
typeof []; // object
typeof null; // object
typeof true; // boolean
typeof(false); // boolean
typeof(123); // number
typeof(NaN); // number
typeof(456n); // bigint
typeof(undefined); // undefined
```

## <mark style="background: #BBFABBA6;">null이 object이다.</mark>

JS1의 오류이다. 당시의 tag type 비트에 연관되어 있으며 이후에 변경하려고 했으나 이미 너무 많은 프로젝트에 녹아들여져 있어서 변경을 취소하였다.

## <mark style="background: #BBFABBA6;">function은 function을 리턴한다.</mark>

객체로 취급되지만 typeof로 찍으면 function을 리턴한다.

```javascript
function sayHi() {
  console.log('Hi!?');
}

typeof sayHi; // function
```

## <mark style="background: #FFF3A3A6;">논리 연산자</mark>

```javascript
console.log(true || false && false); // true
console.log((true || false) && false); // false

console.log('Codeit' || NaN && false); // Codeit
console.log(('Codeit' || NaN) && false); // false
```
## <mark style="background: #BBFABBA6;">&&</mark>

false를 찾아서 있으면 false인 것을 출력 없으면 마지막 걸 출력하는 연산자.

## <mark style="background: #BBFABBA6;">||</mark>

true를 찾아서 있으면 true인 것을 출력 없으면 마지막 걸 출력하는 연산자

우선 순위 : && > ||

## <mark style="background: #FFF3A3A6;">null 병합 연산자</mark>

## <mark style="background: #BBFABBA6;">??</mark>

`null` 혹은 `undefined` 값을 쳐내는 연산자

```javascript
const example1 = null ?? 'I'; // I
const example2 = undefined ?? 'love'; // love
const example3 = 'lsh' ?? 0; // lsh

console.log(example1, example2, example3); // I love lsh
```

논리 연산자( `||` )와 다른 점은 `undefined`와 `null`만 취급한다는 것이다.

## <mark style="background: #FF5582A6;">변수와 스코프</mark>

| 변수 선언 방법 | 스코프 범위 | 재할당 가능 여부 | hoisting 여부        | 중복 선언 가능 여부        |
| -------- | ------ | --------- | ------------------ | ------------------ |
| var      | 함수 스코프 | 가능        | o                  | 가능                 |
| let      | 블록 스코프 | 가능        | x (ReferenceError) | 불가능 (Syntax Error) |
| const    | 블록 스코프 | 불가능       | x (ReferenceError) | 불가능 (Syntax Error) |

## <mark style="background: #FFB86CA6;">함수 스코프의 문제</mark>

if나 while, for같은 블록에서는 변수가 지역 변수가 아니게 되어서 코드 짜기 힘듬 (중복 선언까지 되니깐 더 그럼)

## <mark style="background: #FFB86CA6;">블록 스코프 등장</mark>

블록 단위로 유효범위를 구분하여 코드 안정성 올라감.

## <mark style="background: #FF5582A6;">Function</mark>

## <mark style="background: #FFB86CA6;">Functional Declaration</mark>

일반적인 함수 선언

```javascript
function functionName(){
	//function body
}
```

## <mark style="background: #FFB86CA6;">Functional Expression</mark>

함수 선언을 값처럼 사용하는 방식

```javascript
const functionalExpression1 = function (){}
const functionalExpression2 = function namedfunction () {};
const functionalExpression3 = ()=>{};
```

## <mark style="background: #FFF3A3A6;">Functional Declaration 과 Functional Expression의 차이</mark>

함수 선언식은 미리 EnvironmentRecord가 갖고 있게 된다. (우리의 이해를 위해서 hoisting 이라고 이름을 지음.)
(reference [[2. Execution Context]]) 

```javascript
named() // 'hi' 오류 안 남.
function named(){
	console.log('hi')
}
```

| 방법     | 스코프               |
| ------ | :---------------- |
| 함수 선언식 | 함수 스코프            |
| 함수 표현식 | 할당 받는 변수에 따라 다르다. |

## <mark style="background: #FFF3A3A6;">Named Functional Expression</mark>

함수 표현식으로 함수를 만들 때 이름을 넣어줌  ( name property 문자열로 자동 생성 )

```javascript
const sayHi = function () {
  console.log('Hi');
};
console.log(sayHi.name); // sayHi
```

기명 함수 표현식의 경우 함수 선언식의 이름이 name을 차지하게 된다.

```javascript
const sayHi2 = function sayHi3() {
  console.log('Hi');
};
console.log(sayHi2.name); // sayHi3
```

## <mark style="background: #BBFABBA6;">내부에서 활용 되는 name</mark>

```javascript
let countdown = function(n) { 
	console.log(n); 
	if (n === 0) { 
		console.log('End!'); 
	} else { 
		countdown(n - 1); 
	} 
}; 
countdown(5);
```

해당 name은 해당 함수 내부에서만 사용할 수 있다.

```javascript
const sayHi = function printHiInConsole() {
  console.log('Hi');
};
printHiInConsole(); // ReferenceError
```

익명 함수 내에 변수명을 넣고 쓸 수는 있으나 그 변수를 복사하게 되면 문제가 발생한다. 
(myFunction이 데이터 할당시 받은 value(주소값) 그대로긴 하지만  그 내부의 countdown변수가 null이기 때문에 TypeError(countdown은 function이 아니다) 가 나오게 된다. [[1. Data Type]] )

```javascript
let countdown = function(n) {
  console.log(n);
  if (n === 0) {
    console.log('End!');
  } else {
    countdown(n - 1);
  }
};
let myFunction = countdown;
countdown = null;
myFunction(5); // TypeError
```

아래처럼 Named function으로 만들고 내부적으로 동작하게 만들면 해결된다.

```javascript
let countdown = function printCountdown(n) {
  console.log(n);
  if (n === 0) {
    console.log('End!');
  } else {
    printCountdown(n - 1);
  }
};
let myFunction = countdown;
countdown = null;
myFunction(5); // 정상적으로 동작
```

## <mark style="background: #FFB86CA6;">IIFE (Immediately Invoked Function Expression)</mark>

## <mark style="background: #FFF3A3A6;">형식</mark>

```javascript
(function () {
  console.log('Hi!');
})();

(function (x, y) {
	console.log(x + y);
})(3, 5);
```

이름이 있어도 재사용할 수 없다. 그래서 보통 익명 함수를 사용함.

```javascript
(function sayHi() {
  console.log('Hi!');
})();

sayHi(); // ReferenceError
```

단, 재귀 함수를 내부에서 시키고 싶을 때 named function을 사용한다.

```javascript
(function countdown(n) {
  console.log(n);
  if (n === 0) {
    console.log('End!');
  } else {
    countdown(n - 1);
  }
})(5);

```

프로그램 초기화나 일회성 동작에 많이 활용됨.
```javascript
(function init() {
  // 프로그램이 실행 될 때 기본적으로 동작할 코드들..
})();

const firstName = 'Young';
const lastName = 'Kang';
const greetingMessage = (function () {
  const fullName = `${firstName} ${lastName} `;
  return `Hi! My name is ${fullName}`;
})(); // 리턴값 변수에 활용
```

## <mark style="background: #FFB86CA6;">값으로서의 함수</mark>

```javascript
function printJS(){
  console.log("JS");
}
console.log(printJS); // function
```

함수를 log에 찍어보면 함수 선언이 나온다.
```javascript
console.log(printJS);
```
![[Pasted image 20240327010411.png]]

함수를 dir에 찍어보면 다른 기본형 데이터와는 다르게 객체가 나오게 된다.
```javascript
console.dir(printJS);
```
![[Pasted image 20240327010424.png]]

함수는 객체 형태이기 때문에 어디에서나 할당될 수 있고 호출될 수 있다.

## <mark style="background: #FFF3A3A6;">활용</mark>

## <mark style="background: #BBFABBA6;">객체의 메소드</mark>

```javascript
const myObject = {
  brand: 'Codeit',
  bornYear: 2017,
  isVeryNice: true,
  sayHi: function(name) {
    console.log(`Hi! ${name}`);
  }
};
myObject.sayHi('JavascScript');
```

## <mark style="background: #BBFABBA6;">배열 안의 값</mark>

```javascript
const myArray = [
  'codeit',
  2017,
  true,
  function(name) {
    console.log(`Hi! ${name}`);
  },
];
myArray[3]('Codeit');
```

## <mark style="background: #BBFABBA6;">또다른 함수의 파라미터 (콜백함수)</mark>

```javascript
function makeQuiz(quiz, answer, success, fail) {
  if (prompt(quiz) === answer) {
    console.log(success());
  } else {
    console.log(fail());
  }
};
function getSuccess() {
  return '정답';
};
function getFail() {
  return '오답!';
}; 
const question = '5 + 3 = ?';
makeQuiz(question, '8', getSuccess, getFail);
```

## <mark style="background: #BBFABBA6;">함수에서 함수를 리턴 (고차 함수: High-order Function)</mark>

```javascript
function getPrintHi() {
  return function () {
    console.log('Hi!?');
  };
};
const sayHi = getPrintHi();
sayHi();
```

## <mark style="background: #BBFABBA6;">일급 함수 : First-class Function</mark>

```javascript
function getPrintHi() {
  return function () {
    console.log('Hi!?');
  };
};
getPrintHi()();
```

## <mark style="background: #FFB86CA6;">Parameter and Arguments</mark>

## <mark style="background: #FFF3A3A6;">Parameter</mark>

```javascript
function greeting (param){
	console.log('Hi! My name is ${name}!');
}
```

함수 선언시 소괄호 안에 있는 변수

```javascript
function greeting (param1=25,name){
	console.log('Hi! My name is ${param1} ${name}!');
}

greeting(undefined, 'lsh'); //'Hi! My name is 25 lsh!'
greeting(25,'lsh') //'Hi! My name is 25 lsh!'
greeting('lsh') // Hi! My name is lsh undefined
```

기본값 설정을 할 수 있다. 

```javascript
function sumPlusTwo (x,y=x+2){
return y
}
console.log(sumPlusTwo(2)) // 4
```

기본값 설정을 이용해서 parameter 안에 식을 넣을 수 있다.

## <mark style="background: #FFF3A3A6;">Argument
</mark>

```javascript
function greeting (param){
	console.log('Hi! My name is ${name}!');
}
s```

호출 할때 사용하는 데이터

함수 내부에서 arguments라는 이름의 유사 배열에 전부 저장된다.

```javascript
function greeting(age,name,school) {
  console.log(`Hi! My name is ${age} year's old ${name} in ${school}!`);
  console.log(arguments);
}
greeting(25,'lsh'); // 25 lsh undefined 
greeting(25,'lsh','inu','hi'); // arguments[3]=='hi'
```
![[Pasted image 20240327093424.png]]

## <mark style="background: #FFF3A3A6;">Rest Parameter</mark>

`...parmeter`

함수 내부의 유사배열인 `arguments`와 다르게 배열로써 기능하는 Parameter 

```javascript
function greeting(...args) {
  console.log("Hi! My name is ${age} year's old ${name} in ${school}!");
  console.log(arg);
}
```

유사 배열이 아닌 배열이기 때문에 배열의 모든 메소드를 사용할 수 있다.
( .splice, .forEach, .shift 등등 )

## <mark style="background: #FFB86CA6;">Arrow Function</mark>

### 기본 구조

모든 Arrow Function은 익명 함수이다.

`const functionName = ( parameter )=>{ function body; return result}`
### parameter가 하나일 때 `( )` 생략 가능

`const functionName = parameter => { function body; return result}`

### function body에 return 하나만 있다면 `{ }`와 함께 `return`도  생략 가능

`const functionName = parameter => result`

### return value가 object일 경우 `( )`를 함수 본문의 bracket으로 쓰면 return 생략할 수 있음

`const functionName = parameter => ( { Object Body })`

## <mark style="background: #FFB86CA6;">this</mark>

함수를 호출한 객체를 가리키는 키워드 (기본은 window)

### <mark style="background: #FFF3A3A6;">함수 안의 this</mark>

```javascript
console.log(this);
function printThis(){
	console.log(this);
}

printThis(); //window
```

## <mark style="background: #FFF3A3A6;">함수 표현식에 담긴 this</mark>

```javascript
console.log(this); // window

function printThis(){
	console.log(this);
}

const a = {
	content: 'myObj',
	printThis: printThis,
}

a.printThis(); // a
```

### <mark style="background: #FFF3A3A6;">Arrow Function에서의 this</mark>

```javascript
console.log(this) //window

const printThis = ()=>{
	console.log(this); //window
}

const myObj={
	content: 'myObj',
	printThis: printThis,
}

const otherObj={
	content: 'otherObj',
	printThis: printThis,
}

myObj.printThis(); //window 
other.printThis(); //window
```

Arrow function에서는 선언하기 직전의 this를 할당하기 때문에 메소드에 사용되는 함수 선언 방식은 기본 함수 선언 방식이 더 권장된다.

```javascript
const getFullName = () => `${this.firstName} ${this.lastName}`;
const user = {
  firstName: 'Ted',
  lastName: 'Chang',
  getFullName: getFullName, // function(){} 이거 더 권장 this 때문에
};
console.log(user.getFullName()); // undefined undefined
```

## <mark style="background: #FF5582A6;">JS 문법</mark>

## <mark style="background: #FFB86CA6;">Statements and Expressions</mark>

모든 JS는 Statementss와 Expression으로 구성되어 있다.

## <mark style="background: #FFF3A3A6;">Statements</mark>

어떤 동작을 수행하는 문장 (조건문, 반복문, 할당문, 선언문  ...)

보통 `{ }`으로 끝난다.

```javascript
console.log(if (x < 5) {
  console.log('x는 5보다 작다');
} else {
  console.log('x는 5보다 크다');
}); // Error

const someloop = for (let i = 0; i < 5; i++) {
  console.log(i);
}; //Error

```

## <mark style="background: #FFF3A3A6;">Expressions</mark>

return (하나의 값)을 가지는 코드

보통 `;`로 끝난다.

```javascript
const title = 'JavaScript';
const codeit = {
  name: 'Codeit'
};
const numbers = [1, 2, 3];

typeof codeit // object
title // JavaScript
codeit.name // Codeit
numbers[3] // undefined
```

## <mark style="background: #FFF3A3A6;">Statements, Expressions 둘다 인 것들</mark>

return (하나의 값) 을 가지면서 동시에 어떤 특정 동작을 가진 것들

```javascript
title = 'JavaScript'; // JavaScript
sayHi(); // sayHi 함수의 리턴 값
console.log('hi'); // undefined
```

## <mark style="background: #FFB86CA6;">Conditional Operator(Ternary Operator)</mark>

## <mark style="background: #FFF3A3A6;">구조</mark>

```javascript
condition ? truthy : falsy
```

표현식이기 때문에 조건에 따라 반복문을 돌린다거나 조건에 따라 변수를 선언할 수는 없다.

## <mark style="background: #FFB86CA6;">Spread Syntax</mark>

한 배열의 요소를 흩뿌려놓음.
(Rest Argument와 비슷하게 작동 시킬 수 있다.)

```javascript
const spreads = [1,2,3]
console.log(...spreads) // 1,2,3 (arr형태가 아님.)
console.log(1,2,3); // 1,2,3 (같이 나옴.)
```

| ...을 사용하는 문법명  | 설명                                                     |
| -------------- | ------------------------------------------------------ |
| rest arguments | 말 그대로 argument에서 유사 배열이 아닌 배열 형태로 arguments를 받기 위해서 존재 |
| spread syntax  | 배열이나 객체를 데이터들의 형태로 나눠서 갖고 있기 위해서 필요한 존재                |

## <mark style="background: #FFF3A3A6;">깊은 복사 가능</mark>

```javascript
const arr1=[1,2,3];
const arr2=[...arr1];
arr1.splice(1,1);
console.log(arr1); // [1,3] 
console.log(arr2); // [1,2,3]
```

## <mark style="background: #FFF3A3A6;">쉬운 요소 추가 배열 추가</mark>

 ```javascript
let arr1=[1,2,3];
arr1 = [...arr1,4];
console.log(arr1); // [1,2,3,4]
let arr2=[5,6];
arr1 = [...arr1,...arr2];
console.log(arr1); // [1,2,3,4,5,6]
```

## <mark style="background: #FFF3A3A6;">arguments로도 사용 가능</mark>

```javascript
const arr1 = [1,2,3]
function sum(on,tw,thr){
	return on+tw+thr;
}
sum(...arr1) // 6
```
## <mark style="background: #FFF3A3A6;">Spread Syntax는 값이 아니다.</mark>

```javascript
const arr1=[1,2,3];
const number2 = ...arr1; 
// Uncaught SyntaxError: Unexpected token '...'
```

## <mark style="background: #FFF3A3A6;">객체에도 할당 가능 (ES2018)</mark>

```javascript
const arr1=[1,2,3];
const arr2={ ...arr1 };
```

## <mark style="background: #FFF3A3A6;">객체에서 객체 복사 (ES2018)</mark>

객체끼리의 복사를 편하게 할 수 있다.
```javascript
const codeit = { 
  name: 'codeit', 
};
const codeitClone = { 
  ...codeit, // spread 문법!
};
console.log(codeit); // {name: "codeit"}
console.log(codeitClone); // {name: "codeit"}
```

객체를 다른 객체에 상속시키는 것 같은 행위도 쉽게 할 수 있다.
```javascript
const latte = {
  esspresso: '30ml',
  milk: '150ml'
};

const cafeMocha = {
  ...latte,
  chocolate: '20ml',
}

console.log(latte); // {esspresso: "30ml", milk: "150ml"}
console.log(cafeMocha); // {esspresso: "30ml", milk: "150ml", chocolate: "20ml"}

```

당연하지만 Spread Syntax가 있어도 객체로는 새로운 배열이나 함수의 argument로 사용할 수 없다. ( 객체는 객체로만 사용 가능해서 객체의 블록 안에 spread syntax를 사용해야 한다.)
```javascript
const latte = {
  esspresso: '30ml',
  milk: '150ml'
};
const cafeMocha = {
  ...latte,
  chocolate: '20ml',
}

// 새로운 배열 x
[...latte]; // Error

// 함수 arguments x
(function (...args) {
  for (const arg of args) {
    console.log(arg);
  }
})(...cafeMocha); // Error
```

## <mark style="background: #FFB86CA6;">Modern Object Property</mark>

변수명과 프로퍼티명이 같다면 아래와 같은 식으로 변수 값을 프로퍼티값으로 넣어줄 수 있다.
```javascript
const name = 'lsh';
const birth = '19990107';
const job = 'programmer';

const user ={name, birth, job}; // name:'lsh', birth:'19990107', job:'programmer'

console.log(user); // {name: 'lsh',birth:'19990107',job:'programmer}
```

이는 함수명도 마찬가지이다.
```javascript
function getFullName(){
	return '${this.firstName} ${this.LastName}';
}
const user ={firstName:'l',
			 lastName:'sh',
			 getFullName, //getFullName:getFullName
			};
console.log(user.getFullName()); // l sh
```

메소드 선언시 선언문을 축약할 수 있다.
```javascript
const obj = {
				Method(){} //method: function(){}
			}
```

## <mark style="background: #FFB86CA6;">Computed property name</mark>

## <mark style="background: #FFF3A3A6;">형식</mark>
```javascript
[표현식]:value
```

## <mark style="background: #FFF3A3A6;">호출</mark>

```javascript
const propertyName = 'birth';
const getJob = () => 'job';

const codeit = {
  ['topic' + 'Name']: 'Modern JavaScript',
  [propertyName]: 2017,
  [getJob()]: '프로그래밍 강사',
};

codeit['topicName']
codeit.topicName
codeit[propertyName] 
// 이거 때문에 객체의 프로퍼티 접근에서 대괄호 표기법 쓸려면 표현식이어야 하는 듯
// 일반 프로퍼티를 대괄호 표기법으로 사용하려면 표현식으로 만들고 써야됨. 
// 예를 들면 ['프로퍼티이름'] 이렇게
codeit.birth
codeit[getJob()]
codeit.job
codeit.propertyName // undefined
```

표현식에 넣은 값을 . 표기법으로 표기하면 선언 자체가 다른걸로 되어있으니 undefined가 나오게 된다.

(선언이 `['birth']`로 되어 있는데 `propertyName`을 부르려고 하면 안됨. `[propertyName]`얘도 결국에 `['birth']` 이거잖음)

## <mark style="background: #FFF3A3A6;">예시</mark>
```javascript
const user1 = {['code'+'it']:'hi'}// {'codeit':'hi'}

const user2 = {[function()]:'lsh'}
```

## <mark style="background: #FFB86CA6;">Optional Chaining</mark>

중첩 객체에 접근하는 경우에 존재하지 않는 프로퍼티의 프로퍼티를 찾아서 Error가 발생하는 것을 막기 위해서 사전에 `undefined` 와 `null` 을 찾는 문법
```javascript
function printCatName(user) {
  console.log(user.cat?.name);
  // cat이 undefined나 null이 아니라면 그대로 user.cat.name
  // cat이 undefined나 null이면 undefined 출력
}
```

#### `undefined`나 `null`이 아니라면 그대로 진행

### 맞으면 `undefined` 리턴

## <mark style="background: #FFB86CA6;">Destructuring</mark>

## <mark style="background: #FFF3A3A6;">Array (Destructuring)</mark>

```javascript
const rank =['1','2','3','4'];

const [one,two,three,four]= rank // '1','2','3','4' 

const [one,two,three,...others]=rank; // '1','2','3','4'

const [one,two,three]=rank // '1','2','3',undefined

const [one,two,three,four,five='7']=rank // '1','2','3','4','7' 
// 오직 undefined, 값이 들어가지 않은 경우에만 기본값이 적용된다.
```

## <mark style="background: #BBFABBA6;">변수 값 교환</mark>

```javascript
[v1, v2]=[v2, v1];
```

## <mark style="background: #FFF3A3A6;">Object (Destructuring)</mark>

```javascript
const obj = {
	title: 'hi',
	person: '2',
	age: 25,
	'serial-num':1234,
}

const { title, person }=obj; // title: 'hi', person: '2'

const { title, color }=obj; // title: 'hi', color: undefined

const { title, color='sliver' }=obj; // title: 'hi', color: 'sliver'

const { title, ...rest}=obj; // title: 'hi' , rest={person:'2', age:25,} 남은 프로퍼티를 객체 형태로
// rest parameter에 변경가능

const { title: name, person: number }=obj // name: 'hi', number: '2'
	
const { 'serial-num' : serialNum, title  }=obj // serialNum: 1234, title:'hi'
// 'serial-num'의 형식은 변수명으로 사용할 수 없기 때문에 꼭 : 로 이름 변경을 해줘야 한다.

const propertyName = 'title'
const {[propertyName] : product} = obj;
// computed propertyName을 사용할 수도 있음.
```

## <mark style="background: #FFF3A3A6;">Desturcturing 활용</mark>

### <mark style="background: #BBFABBA6;">함수의 리턴값에 배열을 넣기</mark>
```javascript
function getArray(){
	return ['컴퓨터','냉장고','세탁기'];
}
const [el1,el2,el3] = getArray();
console.log(el1);
console.log(el2);
console.log(el3);
```

### <mark style="background: #BBFABBA6;">함수의 Arguments로 사용 (Array, Object)</mark>
```javascript
function printWinners(...arg){
	const [macbook,ipad, airpods, ...coupon]=arg;
}
// 근데 이거 응용하면
function printWinners([macbook,ipad, airpods, ...coupon]){
}

// 객체
function print(object){
	const {title, color, price}=object;
	}
// 응용
function print({title, color, price}){
}
```
## <mark style="background: #BBFABBA6;">DOM에서의 활용</mark>
```javascript
btn.addEventListener('click',(event)=>{
	event.target.classList.toggle('checked');
})

// {target}=event
btn.addEventListener('click',({target})=>{
	target.classList.toggle('checked');
})

// target:{classList}
// Nested Object Destructuring 나중에 하셈, 지금 내 생각으론 좀 과한듯
btn.addEventListener('click',({target:{classList}})=>{
	classList.toggle('checked');
})
```

## <mark style="background: #FF5582A6;">Module</mark>

## <mark style="background: #FFB86CA6;">조건</mark>

## <mark style="background: #FFF3A3A6;">Module Scope</mark>

브라우저에서 module을 가져오면 안되고 서버에서 html파일을 실행 시켜야 module로 script를 불러올 수 있다.

그리고 그 안에
```html
<script type="module" src="scriptName.js"></script>
```
이렇게 Module Scope를 생성할 수 있다.

Module Scope 안의 것들은 해당 Module 안에서만 동작할 수 있다.

## <mark style="background: #FFF3A3A6;">html 파일에는 하나의 JS파일</mark>

모듈은 서로 연결되어 있기 때문에 모든 것과 연결되어 있는 JS파일 하나만 html에 남기게 된다.

## <mark style="background: #FFB86CA6;">문법</mark>
## <mark style="background: #FFF3A3A6;">import</mark>

module을 import하는 방법

```javascript
import {moduleComponentName} from './modulePath.js'
```

## <mark style="background: #BBFABBA6;">as(import)</mark>

```javascript
import {moduleComponentName as newName} from './printer.js'
```

## <mark style="background: #BBFABBA6;">wildcard import</mark>

```javascript
import * as newObjectName from './modulePath.js'
```

## <mark style="background: #BBFABBA6;">default</mark>

export에서 default로 준거 받기

```javascript
import {default as newName} from './modulePath.js'

import newName, {moduleComponentName as newName} from './printer.js'
```
## <mark style="background: #FFF3A3A6;">export</mark>

module을 export하는 방법

```javascript
export const variableName = value;

export function functionName(argument){ //function body
}
```

## <mark style="background: #BBFABBA6;">골라서 보내기</mark>

```javascript
export { moduleName1, moduleName2}
```

## <mark style="background: #BBFABBA6;">as(export)</mark>

```javascript
export {moduleName as newName}
```

## <mark style="background: #BBFABBA6;">export default</mark>

한 모듈 파일에서 딱 한번만 사용할 수 있는 하나의 대상만 export하는 export 

```javascript
export default { moduleName1, moduleName2}

export default moduleName
```

