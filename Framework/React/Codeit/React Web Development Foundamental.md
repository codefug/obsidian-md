## <mark style="background: #FF5582A6;">create-react-app으로 react 시작</mark>

> // react-app package 설치
> npm init react-app .
> 
> // 포트 열어서 실행
> npm run start
> 
> // 종료
> ctrl + c

Vite로 react 시작

> npm create vite@latest
> npm run dev

## <mark style="background: #FF5582A6;">Vite</mark>

## <mark style="background: #FFB86CA6;">main.jsx</mark>

ReactDOM을 이용해서 Root를 지정, render로 해당 요소를 보낸다.
```jsx
// import React from 'react'
import ReactDOM from 'react-dom/client'
// import App from './App.jsx'
import './index.css'
  
ReactDOM.createRoot(document.getElementById('root')).render(
  // <React.StrictMode>
  //   <App /> // App jsx파일에 있는 App Component를 사용
  // </React.StrictMode>,
  <h1>안녕 리액트!</h1>
)
```

## <mark style="background: #FFB86CA6;">App.jsx</mark>

main.jsx로 보낼 코드들

## <mark style="background: #FFB86CA6;">index.html</mark>

처음 열리는 html 파일

## <mark style="background: #FFB86CA6;">JSX</mark>

html + JS인 JS의 확장 문법

## <mark style="background: #FF5582A6;">React</mark>

## <mark style="background: #FFB86CA6;">규칙</mark>

## <mark style="background: #FFF3A3A6;">prop은 Camel Case로 작성해야한다.</mark>

```jsx
<>
	<button propName="value"></button>
</>
```

비표준 속성 (data-* )의 경우에는 HTML 문법 그대로 작성한다. (- 가능)

```jsx
import ReactDOM from 'react-dom';

ReactDOM.createRoot(document.getElementById("root")).render(
  <div>
    상태 변경:
    <button className="btn" data-status="대기중">
      대기중
    </button>
    <button className="btn" data-status="진행중">
      진행중
    </button>
    <button className="btn" data-status="완료">
      완료
    </button>
  </div>
);
```

## <mark style="background: #FFF3A3A6;">JS에서 사용하는 문법과 겹치는 경우 또다른 이름으로 써야한다.</mark>
## <mark style="background: #BBFABBA6;">ex) class, for</mark>

class => className
for => htmlFor

```jsx
ReactDOM.createRoot(document.getElementById("root")).render(
  <form>
    <label htmlFor="name">이름</label>
    <input id="name" type="text" className="input" />
  </form>
);
```

## <mark style="background: #BBFABBA6;">ex) eventHandler</mark>

onblur => onBlur
onfocus => onFocus
onmousedown => onMouseDown

```jsx
ReactDOM.createRoot(document.getElementById("root")).render(
  <form>
    <label htmlFor="name">이름</label>
    <input id="name" type="text" onBlur="" onFocus="" onMouseDown="" />
  </form>
);
```

## <mark style="background: #FFF3A3A6;">반드시 하나의 태그로 감싸주어야 한다.</mark>

html에 표시 안나게 감싸는 방법으로 Fragment가 있다.

## <mark style="background: #BBFABBA6;">Fragment</mark>

`<Fragment>` `</Fragment>` 로 표시하는데

`<>`  `</>` 으로 줄여서 사용할 수 있다.

```jsx
<>
  <p>안녕</p>
  <p>난 승현이야</p>
</>
```

## <mark style="background: #FFB86CA6;">JSX의 ReactDOM에서 JS 사용하기</mark>

## <mark style="background: #FFF3A3A6;">JS 표현식의 경우</mark>

`{}` 안에 JS코드를 넣으면 된다.

```jsx
// 문자열 사용
const dog = 'happy';
ReactDOM.createRoot(document.getElementById("root")).render(
  <>
  <p>안녕</p>
  <p>난 {dog}야</p>
  </>
);

// 속성 변경
const imageUrl = "www.imagehappy.com";
ReactDOM.createRoot(document.getElementById("root")).render(
  <>
  <img src={imageUrl} alt="제품 사진"/>
  </>
);

// 이벤트 등록
ReactDOM.createRoot(document.getElementById("root")).render(
  <>
  <button onClick={
  ()=>{console.log('곧 도착합니다!')}}>클릭</button>
  </>
);
```

## <mark style="background: #FFB86CA6;">React Element</mark>

jsx에서 html을 담고 있는 js 변수를 React Element라고 한다.

```jsx
const element = <h1>hi react!</h1>;
```

React는 Element를 아래와 같이 JS 객체로 바꾼다.
(render method가 이걸 HTML 형태로 브라우저에 띄우는 것이다.)

```js
{$$typeof: Symbol(react.element), type: "h1", key: null, ref: null, props: {…}, …}
```

## <mark style="background: #FFB86CA6;">React Component</mark>

## <mark style="background: #FFF3A3A6;">규칙</mark>

1. 첫 글자가 무조건 대문자로 시작
2. html을 `return`으로 하는 함수
3. 사용될 때는 html 태그처럼 사용된다. `{}`이것도 안 씀

```jsx
const Hi = ()=>{
  return <h1>안녕 난 리액트얌</h1>;
}

const element = (
  <>
    <Hi />
    <Hi />
    <Hi />
  </>
);

ReactDOM.createRoot(document.getElementById("root")).render(
  element
);
```

## <mark style="background: #FFF3A3A6;">Component 내부의 prop에 JS 넣는 법</mark>

## <mark style="background: #BBFABBA6;">image</mark>

## <mark style="background: #FFB86CA6;">React import img</mark>

```jsx
// Dice.jsx
import diceBlue01 from "./assets/dice-blue-1.svg";
import diceBlue02 from "./assets/dice-blue-2.svg";

const dices = [null, diceBlue01, diceBlue02];

function Dice({ num }) { // destructuring 문법
  return dices[num] ?
    <img src={dices[num]} alt="주사위" />:
    <p>미안해요 그런 주사위는 없네요</p>
}
export default Dice;

//App.jsx
import './App.css'
import Dice from './Dice';

function App() {
  return (
    <>
    <Dice num={2}/>
    </>
  )
}
export default App;
```

1. 해당 이미지 경로를 import
2. return 값이 img tag인 Component 의 src를 그 경로로 지정
3. Component를 App.js로 옮기기 위한 export default 
4. App.js에서 받음. 
5. import `하고 싶은 이름` from 'Dice Component가 있는 파일의 경로'
6. `<하고싶은 이름 />`로 Component를 가져올 수 있다.

## <mark style="background: #FFB86CA6;">Props (properties)</mark>

component에 지정한 속성, props 하나는 prop이라고 부름.

component를 사용하는 곳에서 임의로 attribute를 넣어주면 이것을 object로 만든 후에

component function의 첫 번째 파라미터로 전달된다. (jsx에서 HTML의 attribute들은 object의 property인 것)

```jsx
function App(){
	return (
		<div>
			<Dice color="blue"/> // prop.color="blue"
		</div>
	)
}
```

위의 코드에 의하면 다음의 과정을 거친다.
1. Dice라는 component에 attribute를 지정해서 Dice의 파라미터로 color가 들어가게 동작한다.
2. Dice의 props 객체에는 color라는 property가 생기게 된다.

## <mark style="background: #FFF3A3A6;">children</mark>

textContent를 채우는 prop은 `text` prop 을 이용된다.

```jsx
// Button.jsx
const Button = ({ text }) => {
  return <button>{text}</button>;
};

export default Button;

// App.jsx
...
<div>
    <Button text="던지기" />
    <Button text="처음부터" />
</div>
```

여기서 더 가독성을 위한 방법이 있는데 이는 `children` prop이다.

```jsx
// Button.jsx
const Button = ({ children }) => {
  return <button>{children}</button>;
};

export default Button;

// App.jsx
...
<div>
    <Button>던지기</Button>
    <Button>처음부터</Button>
</div>
```

children에는 tag와 tag 사이의 값을 전부 담겨 있다.

## <mark style="background: #FFF3A3A6;">onChange</mark>

input tag에서 html의 oninput event handler를 대신하는 prop.

event 객체를 파라미터로 받을 수 있으며 콜백 형식으로 jsx의 html로 들어간다.(event.target.value는 기본적으로 string type이다.)

```jsx
const eventHandler = (e)=>{
	e.target.value
}
...
<input onChange={eventHandler} value={variable}></input>
...
```

## <mark style="background: #FFB86CA6;">State</mark>

Component 내부에 변수를 선언할 때는 react 라이브러리에 존재하는 useState안에 있는 State라는 

용어로 대신하는데 이 State는 여러 특징이 존재한다.

```jsx
  import { useState } from "react";
  const [variable, setVariable] = useState('initialValue'); // 초기값을 1로 설정
```

1. variableName의 variable을 사용하려고 하면 prefix로 set을 붙여서 하나를 더 선언한 후 그 set으로 변수의 값을 변경해야 한다.

2. React는 내부적으로 State값이 변경되면 화면을 자동으로 새로 그린다.

## <mark style="background: #FFF3A3A6;">참조형 State</mark>

React에서 Array는 [[1. Data Type#^13b485|불변객체]]로만 취급된다. (Property만 바꾸는 행위(가변적)가 불가능하다.)

set을 이용해서 변수에 할당할 때 arg에 넣는 Array에는 새로운 Array를 넣어야 변경할 수 있다. (통째로 할당해야 한다.)

```jsx
  // const [array,setArray]=useState([]);
  const functionName = () => {

    const nextValue = "value";

    setArray([...array,nextValue]); // 기존 array + nextValue

  };
```

## <mark style="background: #FFF3A3A6;">ex) 주사위게임</mark>

```jsx
import { useState } from "react";
import "./App.css";
import Button from "./Button";
import Dice from "./Dice";

const INITIAL_VALUE='rock';

const random = (n) => {
  return Math.ceil(Math.random() * n);
};

function App() {
  const [num, setNum] = useState(INITIAL_VALUE); // 초기값을 1로 설정
// num이라는 변수를 state를 이용해서 선언
// set으로 할당해야하기 때문에 const로 처리

  const handleRollClick = () => {
    setNum(random(6)); // 변수 데이터 할당
  };

  const handleClearClick = () => {
    setNum(1);
  };

  return (
    <>
      <div>
        <Button onClick={handleRollClick}>던지기</Button>
        <Button onClick={handleClearClick}>처음부터</Button>
      </div>
      <Dice num={num} color="red" />
    </>
  );
}

export default App;
```

## <mark style="background: #FFB86CA6;">Component가 좋은 이유</mark>

html을 재사용하는데 편리하며 구성을 바꾸는 데에도 component 단위로 움직이기 때문에 쉽게 변경할 수 있다.

위의 주사위 예시를 강화해서 Board라는 주사위, 결과, 배열 출력 Component를 따로 만들고 App에 둘을 한번에 처리하는 Button을 장착했다.

```jsx
// Board.jsx
import Dice from "./Dice";
import "./App.css";

function Board({ name, color, array }) {
  const num = array[array.length - 1] || 1;
  const sum = array.reduce((previousValue, currentValue) => {
    previousValue += currentValue;
  }, 0);
  
  return (
    <div>
      <h2>{name}</h2>
      <Dice num={num} color={color} />
      <h2>총점</h2>
      <p>{sum}</p>
      <h2>기록</h2>
      <p>{array.join(" ,")}</p>
    </div>
  );
}

export default Board;

// App.jsx
import { useState } from "react";
import Board from "./Board";
import Button from "./Button";

function App() {
  const [myHistory, setMyHistory] = useState([]);
  const [otherHistory, setOtherHistory] = useState([]);
  
  const random = (n) => {
    return Math.ceil(Math.random() * n);
  };

  const handleRollClick = () => {
    let randomNumber = random(6);
    setMyHistory([...myHistory, randomNumber]);
    randomNumber = random(6);
    setOtherHistory([...myHistory, randomNumber]);
  };

  const handleClearClick = () => {
    setMyHistory([]);
    setOtherHistory([]);
  };

  return (
    <>
      <div>
        <Button onClick={handleRollClick}>던지기</Button>
        <Button onClick={handleClearClick}>처음부터</Button>
      </div>
      <div>
        <Board name="나" color="blue" array={myHistory} />
        <Board name="상대" color="red" array={otherHistory} />
      </div>
    </>
  );
}

export default App;
```

## <mark style="background: #FFB86CA6;">State가 바뀔 때 React가 rendering하는 방식</mark>

state가 바뀌면 react는 다시 rendering을 하게 되는데 이때 virtual DOM이 사용된다.

## <mark style="background: #FFF3A3A6;">Virtual DOM</mark>

JS의 DOM Tree처럼 react에서도 자신들만의 DOM을 만든다.

![[Pasted image 20240408142251.png]]

root부터 시작해서 끝부분까지 퍼져 나가는 모양이다.

![[Pasted image 20240408142332.png]]

만약 state가 변경되면 virtual DOM만 변경되게 된다.

![[Pasted image 20240408142450.png]]

이후 React는 virtual DOM의 변경 사항들을 확인하고 변경 사항들만 모아서 JS DOM에 반영한다.

## <mark style="background: #FFB86CA6;">React inline style</mark>

React에서도 html을 inline style로 꾸며줄 수 있다.

다만, React에서는 style을 객체로 넘겨주고 style prop에 넣어줘야 한다.

```jsx
const styleName = {
	attributeName: "value",
}
...
<element style={styleName}></element>
```

## <mark style="background: #FFF3A3A6;">ex) 버튼의 스타일을 바꾸는 예시</mark>

```jsx
const redStyle = {
  backgroundColor: "red",
  color: "white",
};

const blueStyle = {
  backgroundColor: "blue",
  color: "white",
};

const Button = ({ children, onClick, color }) => {
  const style = color === "red" ? redStyle : blueStyle;
  return (
    <button style={style} onClick={onClick}>
      {children}
    </button>
  );
};

export default Button;
```

## <mark style="background: #FFB86CA6;">React import style</mark>

```jsx
import 'css file path'
```

보통 className에 따라서 스타일이 설정되도록 하면 좋다.

```jsx
import "./Button.css";

const Button = ({ children, onClick, color, className }) => {
  const style = color === "red" ? "redStyle" : "blueStyle";
  return (
    <button className={`${style} ${className}`} onClick={onClick}>
      {children}
    </button>
  );
};

export default Button;
```

`classNames` 라이브러리를 사용해서 class를 배열에 담아서 적용

```jsx
import classNames from 'classnames';

function Button({ isPending, color, size, invert, children }) {
  return (
    <button
      className={classNames(
        'Button',
        isPending && 'pending',
        color,
        size,
        invert && 'invert',
      )}>
     { children }
   </button >
  );
}

export default Button;

```

## <mark style="background: #FFB86CA6;">브라우저가 React를 해석하는 방법</mark>

![[Pasted image 20240409034607.png]]

## <mark style="background: #FFF3A3A6;">Babel</mark>

jsx를 js로 transpiling해주는 프로그램

## <mark style="background: #FFF3A3A6;">bundle</mark>

js파일들을 압축해서 하나의 파일로 만드는 것을 bundling이라고 한다.

