## <mark style="background: #FF5582A6;">Component</mark>

하나의 로직, 생김새를 가진 UI 조각중 하나

markup을 리턴하는 JS function이다.

Component 이름은 대문자로 시작해야하고 HTML tag는 소문자여야 한다.

```jsx
function MyButton() {  
return (  
	<button>I'm a button</button>  
);  
}
```

MyButton을 선언하면 다른 Component에 중첩시킬 수 있다.

```jsx
export default function MyApp() {  
return (  
	<div>  
		<h1>Welcome to my app</h1>  
		<MyButton />  
	</div>  
	);  
}
```

`export default` keyword  는 file 내의 main component를 지칭해준다.

## <mark style="background: #FF5582A6;">JSX</mark>

markup syntax의 한 종류

JSX는 HTML보다 엄격하다.

1. <br /> 처럼 태그를 닫아야 한다.
2. 여러개의 JSX tag를 쓰지 못한다.
	1. 무조건 `<div>...</div>`나 `<></>` wrapper로 묶어야 한다.

```jsx
function AboutPage() {  
	return (  
		<>  
			<h1>About</h1>  
			<p>Hello there.<br />How do you do?</p>  
		</>  
	);  
}
```

JSX로 만들 HTML이 너무 많으면 이걸 써라.
https://transform.tools/html-to-jsx

## <mark style="background: #FF5582A6;">Adding Styles</mark>

React에서는 CSS class를 `className`으로 지칭한다. 이건 HTML class에도 적용된다.

```jsx
<img className="avatar" />
```

```css
/* In your CSS */  
.avatar {  
	border-radius: 50%;  
}
```

이런식으로 스타일을 적용한다.

방법이 정해져 있진 않지만 쉽게는 link tag를 html에 넣거나 framework, build tool이 규정한 걸 따른다.

## <mark style="background: #FF5582A6;">Displaying data</mark>

JSX에서 Curly braces ( `{ }` ) 는 JavaScript로 다시 돌아가게 해서 코드에 JS를 쓸 수 있게 해준다.

```jsx
return (  
	<h1>  
		{user.name}  
	</h1>  
);
```

JSX attribute에서도 JS로 갈 수 있다. 단 `" "` 대신에 `{ }`를 써야 한다. 

```jsx
return (  
	<img  
		className="avatar"  
		src={user.imageUrl}  
	/>  
);
```

style attribute에도 사용 가능하다. style attribute에는 객체를 넣어야 한다.

```jsx
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

## <mark style="background: #FF5582A6;">Conditional rendering</mark>

js와 condition의 동작 방식은 같다.

```jsx
let content;  

// if 사용
if (isLoggedIn) {  
	content = <AdminPanel />;  
} else {  
	content = <LoginForm />;  
}  
return (  
	<div>  
		{content}  
	</div>  
);

// ? 사용
<div>  
	{isLoggedIn ? (  
		<AdminPanel />  
	) : (  
		<LoginForm />  
	)}  
</div>

// else가 필요 없을 경우의 condition
<div>  
	{isLoggedIn && <AdminPanel />}  
</div>
```

## <mark style="background: #FF5582A6;">Rendering lists</mark>

Components의 리스트를 다룰 때는  `for` loop와 Array의 `map()` static method을 사용한다.

```jsx
const products = [  
	{ title: 'Cabbage', id: 1 },  
	{ title: 'Garlic', id: 2 },  
	{ title: 'Apple', id: 3 },  
];

const listItems = products.map(product =>  
	<li key={product.id}>  
		{product.title}  
	</li>  
);  

return (  
	<ul>{listItems}</ul>  
);
```

`li`는 `key` attribute을 갖고 있다. item을 돌릴 때 유니크한 숫자나 문자열로 형제들 끼리를 비교하

기 위해서 사용한다. 보통 데이터베이스 id에서 나오곤 한다. React는 key를 이용해서 item을 삽입, 

삭제, reorder할 때 무슨 일이 일어날 지를 알게 된다.

## <mark style="background: #FF5582A6;">Responding to events</mark>

event handler function을 Component 안에 둬서 event에 반응한다.

```jsx
function MyButton() { 
	function handleClick() {  
		alert('You clicked me!');  
	}  
	return (  
		<button onClick={handleClick}>  
			Click me  
		</button>  
	);  
}
```

`()`를 넣어서 해당 함수를 호출시키면 안된다. 무조건 함수를 넘겨주기만 하면 React가 알아서 이벤트 핸들러로 등록시킨다.

## <mark style="background: #FF5582A6;">Updating the screens</mark>

## <mark style="background: #FFB86CA6;">state</mark>

Component가 정보를 저장하고 보여주기를 원할 때 사용하는 것이 `state`이다.

1. import useState from React

```jsx
import { useState } from 'react';
```

2. component 내에 state 변수를 선언한다.

```jsx
function MyButton() {  
	const [count, setCount] = useState(0);  
	// ...
```

current state (count)와 그걸 업데이트하는 함수 (setCount) 를 얻었다.

아무거나 넣어도 되긴하지만 전통적으로 [something, setSomething]의 형태이다.

useState()안에 있는 값( 예시에선 0 )은 current state의 초기 값을 뜻한다.

```jsx
function MyButton() {  
	const [count, setCount] = useState(0);  
  
	function handleClick() {  
		setCount(count + 1);  
	}  
  
	return (  
		<button onClick={handleClick}>  
			Clicked {count} times  
		</button>  
	);  
}
```

위 코드를 실행하면 button을 누를 시 count가 증가하게 된다.

외부에서 해당 component를 여러번 render하더라도 state는 component 각각이 갖고 있다.

```jsx
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

![[Pasted image 20240409140308.png]]

## <mark style="background: #FF5582A6;">Using Hooks</mark>

`use`로 시작하는 functions을 Hooks라고 한다. ( useState 역시 React built-in Hook이다.)

기존의 것과 합쳐서 나만의 Hooks을 만들 수 있다.

Hook은 다른 function보다 엄격하다.

component들의 맨 위에서만 호출할 수 있다.
(만약 useState를 조건문이나 반복문에 넣고 싶으면 Component 하나를 만들어서 그걸 거기에 넣어야 한다.)

## <mark style="background: #FF5582A6;">Sharing data between components</mark>

아까 버튼의 예시를 보자.

![[Pasted image 20240409140746.png]]

다른 MyButton에 영향을 안 주도록 되어있었다.

만약 MyButton들이 count를 공유하게 하고 싶으면 state를 가장 가까운 모든 MyButton들을 포함하

는 component로 보내야 한다.

![[Pasted image 20240409141019.png]]
코드로 보자.

1. state를 MyButton에서 MyApp으로 "upward"

```jsx
export default function MyApp() {  
	const [count, setCount] = useState(0); // state 올리기.
	  
	function handleClick() { // 함수 올리기
		setCount(count + 1);
	} 
	  
	return (  
		<div>  
			<h1>Counters that update separately</h1>  
			<MyButton />  
			<MyButton />  
		</div>  
	);  
}  
	  
function MyButton() {  
// ... we're moving code from here ...  
}
```

2. state를 MyApp에서 각각의 MyButton으로 보낸다. JSX의 curly braces ( `{ }` )를 이용해서 정보를 MyButton으로 전달한다.

```jsx
export default function MyApp() {  
	const [count, setCount] = useState(0);  
	  
	function handleClick() {  
		setCount(count + 1);  
	}  
	  
	return ( 
		<div>  
			<h1>Counters that update together</h1>  
			<MyButton count={count} onClick={handleClick} />  
			<MyButton count={count} onClick={handleClick} />  
		</div>  
	);  
}
```

이렇게 내려보낸 정보를 **props** 라고 한다.  이제 MyApp component는 `count` state와 `handleClick` 
이벤트 핸들러를 갖고 있고 props로써 두개 다 각 button들로 보낸 상태이다.

3. 부모 Component에서 보낸 props를 읽기 위해 MyButton을 바꾼다.

```jsx
function MyButton({ count, onClick }) {  	
	return (  
		<button onClick={onClick}>  
			Clicked {count} times  
		</button>  
	);  
}
```

이렇게 하면 버튼을 클릭하면 handleClick이 실행되고 새로운 count가 prop으로써 통과된다. 이걸 "lifting state up"이라고 한다.

전체 코드
```jsx
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```