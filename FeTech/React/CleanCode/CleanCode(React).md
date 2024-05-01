## <mark style="background: #FF5582A6;">State</mark>

불필요한 state는 만들지 않는 것이 좋다.

## <mark style="background: #FFB86CA6;">관련 용어들</mark>
1. 컴포넌트 상태
2. 전역 상태
3. 서버 상태
4. 상태 변경
5. 상태 최적화
6. 렌더링 최적화
7. 불변성
8. 상태 관리자

## <mark style="background: #FFB86CA6;">올바른 초기값 설정</mark>

가장 먼저 렌더링될때 순간적으로 보여질 수 있는 값

올바르게 초기값을 설정할 경우 해결할 문제 
1. 렌더링 이슈, 무한 루프, 타입 불일치로 의도치 않는 동작 => 런타임 에러
2. 넣지않으면 undefined => 의도치 않은 동작
	1. 상태를 지울 때도 초기값을 넣어야 원상태로 돌아간다.
	2. 빈값, null 처리할때 불필요한 동작 없어도 됨.

## <mark style="background: #FFB86CA6;">업데이트되지 않는 변수</mark>

```jsx
export const component = ()=>{
const INFO = { // 업데이트 되지 않기 때문에 문제가 된다.
	a:"a",
	b:"b",
};

return <MyComp info={INFO} />
};
```

component가 렌더링 될때마다 계속 참조 되며 계산해야될 시기, 기억해야할 시기에 대한 로직이 들어 있지 않다.

두가지 해결점
1. 외부로 이동시킨다.
2. state로 만든다.

## <mark style="background: #FFB86CA6;">Flag</mark>

Flag : 프로그래밍에서 주로 특정 조건 혹은 제어를 위한 조건을 boolean으로 나타내는 값 (그냥 개발 용어임)

```jsx
import { useState ,useEffect } from "react";
  
// 정리 안한 버전
export const Component = () => {
  const [isLogin, setisLogin] = useState(false); // 불필요한 state
  
  useEffect(() => 
    if (a) {
      setisLogin(true);
    }
    if (b) {
      setisLogin(true);
    }
  }, [a, b]);
};
```

```jsx
// 정리 버전
export const Component = ()=>{
  const isLogin = a||b; // flag 변수
}
```

불필요한 state를 만들지 않기 위해서 boolean을 다루는 변수의 경우 따로 useEffect로 관리하지 않고 어짜피 랜더

링 될때 변수는 계속 다시 판단되기 때문에 (오른쪽의 isLogin이 a||b를 랜더링마다 판단하듯이) Flag 변수로 만들어

서 유지보수 쉽게 만들 수 있다.

## <mark style="background: #FFB86CA6;">불필요한 state 제거</mark>

```jsx
import { useEffect, useState } from "react";

// 정리 안된 버전
const [userList, setUserList] = useState(MOCK_DATA);
const [complUserList, setComplUserList] = useState(MOCK_DATA);  
useEffect(() => {
  const newList = complUserList.filter((user) => user.completed === true);
  setUserList(newList);
}, [userList]);
```

```jsx
// 정리 버전
const complUserList = complUserList.filter((user) => user.completed === true);
// 업데이트 되는 변수이기 때문에 랜더링때마다 고유의 계산된 값 생긴다.
```

## <mark style="background: #FFB86CA6;">useState 대신에 useRef</mark>

컴포넌트의 전체적인 수명과 동일하게 지속된 정보를 일관적으로 제공해야 하는 경우

렌더링 프로세스와 상관없이 값을 가변적으로 저장하고 싶은 경우 `useRef` 사용
(컴포넌트의 생명주기와 동일한 리렌더링되지 않는 상태를 만들 수 있다)

```jsx
import { useState, useEffect, useReducer } from "react";

// 정리 안된 버전
export const Component = () => {
// Mount : Page에 Dom을 올리는 행위
  const [isMount, setIsMount] = useState(false);

  // 불필요한 렌더링을 계속 반복한다.
  useEffect(() => {
    if (!isMount) {
      setIsMount(true);
    }
  }, [isMount]);
};
```

```jsx
// 정리 버전
export const Component = () => {
// 렌더링을 유발하지 않는 가변 컨테이너
  const isMount = useRef(false);

  // 처음 렌더링 때만 실행한다.
  useEffect(() => {
    isMount.current = true;

    return () => (isMount.current = false);
  }, []);
};
```

## <mark style="background: #FFB86CA6;">연관된 상태 관리하기</mark>

## <mark style="background: #FFF3A3A6;">연관된 상태 단순화하기</mark>

```jsx
import { useState } from "react";

// 정리 안된 버전
const [isLoading, setIsLoading] = useState(false);
const [isFinish, setIsFinish] = useState(false);
```

일일이 하나씩 추가해야 되고 가독성도 떨어진다.

```jsx
// 정리 버전 (연관된 상태 단순화)
// KISS(Keep It Simple Stupid)
const PROMISE_STATE = {
  INIT: "init",
  LOADING: "loading",
  FINISH: "finish",
};

const [promiseState, setPromiseState] = useState(PROMISE_STATE.INIT);

// 이후 이런식으로 사용한다.
setPromiseState(PROMISE_STATE.LOADING);
// setPromiseState("loading") 와 같다.

//...

if (promiseState === PROMISE_STATE.LOADING) return <LoadingComponent />;
```

객체로 state들을 묶을 수도 있지만 문자열로 관리할 수도 있다.

해당 문자열을 가졌을 때 리턴 치는 로직을 쓰면 된다.

## <mark style="background: #FFF3A3A6;">연관된 상태 객체로 묶기</mark>

```jsx
// 정리 안된 버전
const [isLoading, setIsLoading] = useState(false);
const [isFinish, setIsFinish] = useState(false);
const [isError, setIsError] = useState(false);
```

```jsx
// 정리 버전 (연관된 상태 객체 형태로 만들기)
const [fetchState, setfetchState] = useState({
  isLoading: false,
  isFinish: false,
  isError: false,
});
  
// 이후 이런식으로 사용한다.
setfetchState((prevState) => ({ ...prevState, isLoading: true }));
  
setfetchState((prevState) => ({
  ...prevState,
  isLoading: false,
  isError: true,
}));
  
//...
if (fetchState.isLoading) return <LoadingComponent />;
```

연관되고 서로 영향을 주는 state의 경우 객체 형태로 묶어서 관리하는게 좋다.

## <mark style="background: #FFF3A3A6;">연관된 상태 useState 대신 useReducer로 refactoring</mark>

```jsx
// 정리 안된 버전 (일일이 state로 관리한다.)
const [isLoading, setIsLoading] = useState(false);
const [isFinish, setIsFinish] = useState(false);
```

```jsx
// 정리된 버전
const INIT_STATE = {
  isLoading: false,
  isSuccess: false,
  isFail: false,
};

const ACTION_TYPE = {
  isLoading: "FETCHLOADING",
  isSuccess: "FETCHSUCCESS",
  isFail: "FETCHFAIL",
};

const reducer = (state, action) => {
  switch (action.type) {
    case "FETCHLOADING":
      return {
        ...state,
        isLoading: true,
      };
    case "FETCHSUCCESS":
      return {
        ...state,
        isSuccess: true,
      };
    case "FETCHFAIL":
      return {
        ...state,
        isFail: true,
      };
    default:
      return INIT_STATE;
  }
};

const [state, dispatch] = useReducer(reducer, INIT_STATE);
//state의 setter함수가 dispatch임.
// 첫번째 인자인 reducer에는 조작하는 함수
// 두번째 인자는 초기값

//...
dispatch({ type: ACTION_TYPE.isLoading });
// dispatch({ type: "FETCHLOADING"}); > reducer를 타고 action.type==="FETCHLOADING" 통과
```

## <mark style="background: #BBFABBA6;">Custom Hook으로 정리</mark>

```jsx
// 정리되지 않은 모습
const [state, setState] = useState();

useEffect(() => {
  const fetchData = () => {
    setState(data);
  };
  fetchData();
}, []);
  
if (state.isLoading) return <LoadingComponent />;
if (state.isFail) return <FailComponent />;
```

```jsx
// 정리된 모습 (로직에 관련된 부분은 Custom Hook으로 추출한 모습)
// 화면에 렌더링 되는 JSX를 제외하고 로직에 관련된 부분만 추출

// useFetchData.jsx
const INIT_STATE = {
  isLoading: false,
  isSuccess: false,
  isFail: false,
};

const ACTION_TYPE = {
  isLoading: "FETCHLOADING",
  isSuccess: "FETCHSUCCESS",
  isFail: "FETCHFAIL",
};

const useFetchData = (url) => {
  const [state, dispatch] = useReducer(reducer, INIT_STATE);

  useEffect(() => {
    const fetchData = async () => {
      dispatch({ type: ACTION_TYPE.isLoading });

      await fetch(url)
        .then(() => {
          dispatch({ type: ACTION_TYPE.isSuccess });
        })
        .catch(() => {
          dispatch({ type: ACTION_TYPE.isFail });
        });
    };

    fetchData();
  }, [url]); // url에 의존성 있기 때문에 추가

  // 객체 형태로 return
  return state;
};

// Component.jsx
const { isLoading, isFail, isSuccess } = useFetchData("url");

if (isLoading) return <LoadingComponent />;
if (isFail) return <FailComponent />;
```

## <mark style="background: #FFB86CA6;">이전 상태 활용</mark>

```jsx
setState((prevState)=>prevState+1); // setState( 실행 당시 값 + 1 )
// updater function이라고 한다.
```

## <mark style="background: #FF5582A6;">Props</mark>

## <mark style="background: #FFB86CA6;">불필요한 Props 복사 및 연산</mark>

```jsx
import { useMemo } from "react";

// 정리 안된 버전
// 불필요한 Props 복사를 지양하자.
function CopyProps1({ value }) {
  // 간단하긴 한데 이미 조작된 value에만 해당되는 구조.
  return <div>{value}</div>;
}

function CopyProps2({ value }) {
  // 상태로 만들 필요가 없다.
  const [copyValue] = useState(값비싸고_무거운_연산(value));
  return <div>{copyValue}</div>;
}
```

```jsx
//정리 버전
function CopyProps3({ value }) {
  // 렌더링될 때마다 연산이 실행된다. (무겁지 않으면 괜찮다.)
  const copyValue = 값비싸고_무거운_연산(value);

  return <div>{copyValue}</div>;
}

function CopyProps4({ value }) {
  // value가 바뀔때만 연산을 진행한다.
  const [copyValue] = useMemo(() => 값비싸고_무거운_연산(value), [value]);

  return <div>{copyValue}</div>;
}

// 사실 컴포넌트 들어오기 전에 연산이 처리되는 것이 제일 좋다.
```

## <mark style="background: #FFB86CA6;">중괄호 사용법</mark>

```jsx
// 문자열의 경우 중괄호 없이 해도 렌더링 잘됨
<header className="clean-header" /> // "clean-header"

// 나머지는 중괄호 사용해야됨.
<header value={1} // true, [] , ()=>{}, 1+2, {} , logical expression />
```

## <mark style="background: #FFB86CA6;">Shorthand Props</mark>

```jsx
function ShorthandProps({ isDarkMode, isLogin, ...props }) {
  return (
    <header
      className="clean-header"
      title="Clean Code React"
      isDarkMode={isDarkMode}
      isLogin={isLogin}
      hasPadding // boolean이고 true인게 확정이면 value를 스킵하고 key만 적어도 된다.
      isFixed
      isAdmin
    >
      <ChildComponent {...props} />{" "} // props를 전부 가져온다. 일일이 안 적어도 됨.
    </header>
  );
}
```

## <mark style="background: #FFB86CA6;">Props Naming</mark>

함수에 매개변수로 넘긴다. 인자들을 주고받으면서 props로 통신한다.

Component일 경우 PaskalCase

그냥 속성일 경우 camelCase

boolean이고 true일 경우 value 생략

```jsx
<Children
	className={className} // 그냥 속성은 camel
	Component={Component} // Component는 Paskal
	boolean // 무조건 true일때 사용
```

## <mark style="background: #FFB86CA6;">inline style 주의하기</mark>

```jsx
// 컴포넌트 안에 넣으면 재랜더링 시 재 선언되기 때문에 밖으로 뺌.
const MyButtonStyle = {
  warning: { backgroundColor: "yellow", fontSize: "14px" },
  danger: { backgroundColor: "red", fontSize: "24px" },
};
  
function InlineStyle() {
  return (
    <>
      <button style={MyButtonStyle.warning}>warning</button> // js코드를 넣는 {}을 활용해서 정리
      <button style={MyButtonStyle.danger}>danger</button>
    </>
  );
}
```

## <mark style="background: #FFB86CA6;">CSS in JS</mark>

style관련 코드를 따로 분리해야  재렌더링 시 다시 돌지 않는다.

객체형태로 component에 저장하면 좋다. (취향 차이인듯)

## <mark style="background: #FFB86CA6;">객체 Props 지양하기</mark>

```jsx
import { useMemo } from "react";
  
/**
 * 객체 Props 지양하기
 *
 * - 변하지 않는 값일 경우 컴포넌트 외부로 드러내기
 * - 필요한 값만 객체를 분해해서 Props로 내려준다.
 * - 복잡한 연산일 때만 useMemo()로 계산된 값을 기억한다.
 * - 컴포넌트를 더 평탄하게 나누면 나눌 Props도 평탄하게 나눠서 내릴 수 있다.
 */
function SomeComponent({heavyState}) {
  const [propArr, setPropArr] = useState(["hello", "hello"]);
  
  // 복잡한 연산의 경우 useMemo를 사용해서 처리한다.
  const computedProp = useMemo(() => ({ heavyState }), [heavyState]);
  
  return (
    <ChildComponent
      hello="world"
      hello2={propArr.at(0)} // 배열 destructuring을 이용한 방법
      computedProp={computedProp} // useMemo
    />
  );
}
```

## <mark style="background: #FFB86CA6;">HTML  ATTRIBUTE 주의하기</mark>

js property, HTML attribute 이름이 겹칠 경우 ...arg를 이용해서 함수를 구현한다.

![[Pasted image 20240418182357.png]]

## <mark style="background: #FFB86CA6;">Spread 연산자 주의점</mark>

![[Pasted image 20240418182730.png]]

관련 있는 props를 명시함으로써 추후에 유지보수가 쉽도록 할 수 있다.

## <mark style="background: #FFB86CA6;">많은 Props 분리</mark>

![[Pasted image 20240418183138.png]]

결과보다는 일단 실행이 중요하다 > 분리의 대상을 구하자.

Tanstack Query, Form Library, 상태 관리자, Context API, Composition 같은걸 쓰기 전에 분리부터 해라.

1. One Depth 분리(사진 처럼)
2. 확장성을 위한 분리를 위해 도메인 로직을 다른 곳으로 모아넣는다. (Context API 혹은 Custom hook으로)

## <mark style="background: #FFB86CA6;">Props 객체 분해해서 받기 (destructuring)</mark>

Props 객체 자체를 파라미터로 받아버리면 불필요한 Props를 가져올 수 있고 

또한 상위 컴포넌트에서 Props를 변경했을 때 불필요한 컴포넌트 리랜더링이 일어날 수 있다.

![[Pasted image 20240418183816.png]]

## <mark style="background: #FF5582A6;">Component</mark>

## <mark style="background: #FFB86CA6;">Fragment 지향하기</mark>

실제로 랜더링되지 않는 하나로 감싸는 element

```jsx
return (
	<>
		somehting
	</>
)
```

key가 필요할 경우 React.Fragment로 다 적는다.

```jsx
return (<React.Fragment key={uniqueSomething}>
		something
		</React.Fragment>)
```

## <mark style="background: #FFB86CA6;">잘못된 Fragment 사용</mark>

```jsx
// 정리되지 않은 버전
return (
  <>
    <div>
      <ChildA />
      <ChildB />
    </div>
  </>
);
return <>hello world</>;
return <h1>{isLoggedIn ? "User" : <></>}</h1>;
```

```jsx  
// 정리된 버전
return ( 
//div가 있을 경우 굳이 Fragment를 사용하지 않아도 된다.
  <div>
    <ChildA />
    <ChildB />
  </div>
);
//문자열이나 배열일 경우 그냥 리턴 해도 된다.
return "hello world";

//<></>은 내부적으로 null이다. (세번째 예시)
return {isLoggedIn && <h1>User</h1>}; 
// or 그냥 isLoggedIn && <h1>User</h1>해도 됨 한줄이라
```
## <mark style="background: #FFB86CA6;">컴포넌트 네이밍</mark>

![[Pasted image 20240418185714.png]]

1. 기본 HTML 요소는 lower case
2. 컴포넌트는 PaskalCase
3. 파일명은 kebab case를 사용하는 경우도 있지만 파일 안에서 컴포넌트는 PaskalCase 사용하는게 좋다.

## <mark style="background: #FFB86CA6;">JSX 컴포넌트 함수로 반환 금지</mark>

![[Pasted image 20240418195056.png]]

<TopRender />를 써야 하는 이유 2가지

1. 함수와의 구분
2. prop 전달
3. scope를 알아보기 어렵다.

## <mark style="background: #FFB86CA6;">컴포넌트 내부의 inner 컴포넌트 선언 금지</mark>

```jsx
// 정리 안된 버전
function OuterComponent() {
  function InnerComponent() {
    return <div>Inner Component</div>;
  }
  return (
    <div>
      <InnerComponent />
    </div>
  );
}
  
// 정리된 버전
const InnerComponent = () => {
  return <div>Inner Component</div>;
};
  
function OuterComponent() {
  return (
    <div>
      <InnerComponent />
    </div>
  );
}
```

금지 이유
1. 결합도가 증가
	1. 구조적으로 스코프적으로 종속된 개발이 된다.
	2. 나중에 확장성이 생겨서 분리될 때 힘듬
2. 성능 저하
	1. 상위 컴포넌트 리렌더될 때마다 하위 컴포넌트 재생성

## <mark style="background: #FFB86CA6;">displayName</mark>

react dev tool에서 볼때 컴포넌트의 이름을 지정 
```jsx
export default () => {}; // 익명함수로 되어 있으면 displayName이 없음
  
// 지정하는 법
함수를 담은 변수.displayName = "하고싶은 value";
```

## <mark style="background: #FF5582A6;">Rendering</mark>

## <mark style="background: #FFB86CA6;">JSX에서의 공백처리</mark>

```jsx
<div>띄어야 하는 공백{" "}{"+ <<이렇게 사용한다."}</div>
```

{" "}으로 안에 있는 공백을 처리한다.

## <mark style="background: #FFB86CA6;">0은 JSX에서 유효한 값이다.</mark>

true나 false로 판단해야 실수 없이 작동시킬 수 있다.

## <mark style="background: #FFB86CA6;">리스트 내부의 key</mark>

 왜 li에 key가 필요한가?

1. 리액트가 가상 돔에서 변경,삭제된 부분을 알기 위해서이다.

어떤 key를 넣어야 할까?

new Date()나 uuid같은 걸 렌더링 계속하는 컴포넌트에 넣으면 안된다.

item.id을 넣어도 되고(서버에서 처리하거나 새로운 아이템을 받을 때 특정 고유한 값을 넣는다.)

```jsx
const handleAddItem = (value)=>{
	setItems((prev)=>{
	...prev,
	{
		id:crypto.randomUUID(), // 따로 특정 고유한 값을 넣어줌.
		value: value,
	},
});
```

## <mark style="background: #FFB86CA6;">Raw HTML</mark>

만약 게시판이나 에디터처럼 Raw한 데이터를 본다고 했을 때 주의할 점이 있다.

```jsx
/**
 * 안전하게 Raw HTML 다루기
 *
 * 1. 렌더링될 데이터
 * 2. 유저가 다시 입력 모드로 수정할 수 있는 데이터
 * input이나 textarea있어야 되는데?
 *
 */
const SERVER_DATA = "<p>some raw html</p>";
  
function DangerouslySetInnerHTMLExample() {
  const post = {
    // XSS(Cross Site Scripting) 악성 스크립트 공격
    content: `<img src="" onerror='alert("you were hacked")'>`,
  };
  
  const markup = { __html: "<p>some raw html</p>" };
  
  // XSS에 취약
  return <div>{markup}</div>;
  
  // 데이터 보안성 향상에는 dompurify library, HTML 공식 API도 있다.
  // eslint-plugin-risXSS로 보안성 실시간 감시 가능
  const sanitizeContent = { __html: DOMPurify.sanitize(SERVER_DATA) };
  
  setContentHTML(DOMPurify.sanitize(SERVER_DATA)); //보안성이 강화된 Content를 넣는다.
  
  return <textarea>{contentHTML}</textarea> // textarea의 기본값으로 설정
  
  // 살균된 content를 담는다.
  return <div dangerouslySetInnerHTML={sanitizeContent} />; // 그냥 보여줄때는 이렇게
}
```

## <mark style="background: #FF5582A6;">Hooks</mark>

## <mark style="background: #FFB86CA6;">useEffect() 기명 함수와 함께 사용하기</mark>

```jsx
useEffect(function 로직을알수있게해주는함수명(){
	//some logic
},[state])
```

console.log, report, monitoring, React Devtools 등등 디버깅 도구를 사용할 때 로그로 에러를 확인하게 되는데 기명 함수로 쓰게 되면 에러를 확인하는데 직관적으로 알 수 있다.

## <mark style="background: #FFB86CA6;">한가지 역할만 수행하는 useEffect</mark>

SRP (Single Responsive Principle) 한가지 역할만 하는 걸 만들자
(리액트의 함수, 컴포넌트, useEffect를 포함한다.)

useEffect가 한가지 역할만 하는지 확인하는 법
1. 기명 함수 사용
2. dependency Arrays에 적은 관찰 대상

```jsx
// 정리 전 (두가지 역할을 하고 있음. 불필요한 작업함.)
useEffect(()=>{
	redirect(newPath);
	const userInfo = setLogin(token);
},[token,newPath])
```

```jsx
// 정리 후
useEffect(()=>{
	redirect(newPath);
	
	if (options){
		//부가적인 로직 <= 추가 동작해도 이상이 없고 부작용이 생길 일이 없다.
	}
	
},[newPath,options])

useEffect(()=>{
	const userInfo = setLogin(token);
	
	if (options){
		//부가적인 로직 <= 추가 동작해도 이상이 없고 부작용이 생길 일이 없다.
	}
	
},[token,options])
```

## <mark style="background: #FFB86CA6;">Custom Hooks 만들 때 주의 사항</mark>

```jsx
function ReturnCustomHooks() {
  // 순서를 맞춘다. state와 setter
  const [value, setValue] = useSomeHooks(true);
  
  // 하나의 value일 경우 해당 state만 리턴 받는다.
  const oneValue = useSomeHooks();
  
  // 배열구조분해 할당할 때 리턴하는 것을 배열이 아닌
  // 객체로 처리하면 쓸 것들만 받기 쉽다.
  const {first:firstValue, third:thirdValue, rest:restValue}=useSomeHooks():
}
```

## <mark style="background: #FFB86CA6;">useEffect() 콜백으로 비동기 함수 불가</mark>

```jsx
useEffect(()=>{
	const fetchData = async ()=>{
		const result = await someFetch();
	}
	fetchData();
},[])
```

그래서 콜백 안에 async를 넣는 식으로 구성을 짜야한다.

## <mark style="background: #FFB86CA6;">import react</mark>

v17부터 react-runtime이 돌면서 import React from 'react' 안해도 된다.

## <mark style="background: #FFB86CA6;">SPA에서의 새로고침</mark>

window.location.reload()을 사용하면 SPA 입장에서는 앱을 완전히 종료하고 다시 실행한다.
(로그인 기능 만들 때 자주 실수하는 요소임)

브라우저에 로컬 스토리지 같은 곳에서 state를 저장해서 꺼내오면 사용자 입장에서는 데이터가 그대로가 된다.

번외.
(devtool로 doc누르고 preview확인하면 SPA인지 확인할 수 있음. 기본 html볼 수 있음.)

## <mark style="background: #FFB86CA6;">Primitive UI</mark>

도메인 네임보다는 Semantic한 Primitive UI를 묘사하는게 좋다. (생김새를 기준으로 Component 이름을 짓는게 좋다.)
ex) Box, Circle, List, Square

Radix UI나 chakra UI를 보면 대충 감이 올 수 있다.

타입스크립트에서 HTMLButton을 interface에서 확장 시켜서 활용할 수 있다.

```jsx
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLBUTTONELEMENT>{
		...
}

const Button = (props: ButtonProps)=>{
	return <button>
		{children}
	</button>
}
```