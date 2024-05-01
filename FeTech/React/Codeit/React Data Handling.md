## <mark style="background: #FF5582A6;">Array Rendering</mark>

## <mark style="background: #FFB86CA6;">map rendering</mark>

map으로 JSX를 리턴하면 JSX를 여러개 둔 것처럼 작동한다.

```jsx
function ComponentName({items}){
	return <ul>{items.map((item)=>return <li>{item.property}</li>)}</ul>
}
```

## <mark style="background: #FFF3A3A6;">ex) li element를 map로 jsx 배열로 만듬.</mark>

```jsx
function FoodListItem({ item }) {
  const { imgUrl, title, calorie, content } = item;

  return (
    <div>
      <img src={imgUrl} alt={title} />
      <div>{title}</div>
      <div>{calorie}</div>
      <div>{content}</div>
    </div>
  );
}

function FoodList({items}) {
  return <ul>{items.map((item)=><li>{FoodListItem({item})}</li>)}</ul>;
}

export default FoodList;

// App.jsx
import FoodList from './FoodList';
import items from '../mock.json';

function App() {
  return (
    <div>
      <FoodList items={items} />
    </div>
  );
}

export default App;
```

## <mark style="background: #FFB86CA6;">sort 활용</mark>

state를 이용해서 sort를 다양한 방식으로 바꿀 수 있다.

```jsx
const [order, setOrder] = useState("createdAt");
const sortedItems = items.sort((a,b)=> b[order] - a[order]);
```

onClick handler 같은 곳에 setOrder를 넣어서 order를 변경하게 하면 실시간으로 정렬의 종류

를 변경할 수 있다. (어떤 property를 기준으로 정렬하는지 변경할 수 있다.)

```jsx
import { useState } from "react";
import items from "./pokemons";

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  const [direction, setDirection] = useState(1);
  
  const handleAscClick = () => setDirection(1);
  const handleDescClick = () => setDirection(-1);
  
  const sortedItems = items.sort((a, b) => direction * (a.id - b.id));
  
  return (
    <div>
      <div>
        <button onClick={handleAscClick}>도감번호 순서대로</button>
        <button onClick={handleDescClick}>도감번호 반대로</button>
      </div>
      <ul>
        {sortedItems.map((item) => (
          <li key={item.id}>
            <Pkemon item={item} />
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

위 코드에서 direction만 바꿔도 sort까지 되는 이유는 변경 사항들에 대한 rendering이 통째로 되기 때문이다.

## <mark style="background: #FFB86CA6;">filter 활용</mark>

Array 메소드중 하나로 콜백이 true 인 것들만 filtering해서 리턴할 배열에 push

```jsx
const handleDelete = (id) => {
	const nextItems = items.filter((item) => item.id !== id);
	setItems(nextItems);
};
```

위 코드를 이벤트 핸들러에 넣을때 주의할 점은 콜백함수에는 함수 자체가 들어가야한다는 것이다.

함수 자체가 들어가면 arg에 event  객체가 무조건 들어가기 때문에 다른 arg를 쓰기 위해서는 따로 

함수로 다시 감싸줘야한다.

```jsx
()=>handleDelete(item.id);
```

## <mark style="background: #FFB86CA6;">key attribute</mark>

React가 리스트의 각 요소들을 인식할 수 있게 해주는 유일한 값으로써 리스트을 다룰 때는 항상 들어가야 한다.

```jsx
<li key="value"></li>
```

수정, 삭제시 해당 element를 특정화 하기 위해서 React에서 사용하는 방식이다.

## <mark style="background: #FF5582A6;">useEffect</mark>

처음 rendering 시에만 실행 후, deps의 value가 변경되면 effect를 실행한다.

```js
useEffect(effect /*call back 함수*/, deps);
```
## <mark style="background: #FF5582A6;">React fetch</mark>

fetch는 데이터를 받아서 Promise 형태로 내보내는 JS문법이다. 
[[Web Development JavaScript - chapter 2 Asynchronous#<mark style="background FFF3A3A6;">fetch</mark>|fetch]]

## <mark style="background: #FFB86CA6;">ex) React에서 활용</mark>

```jsx
export async function getReviews() {
	try{
		const response = await fetch("url");
		const body = await response.json();
		return body;
	}catch(error){
		//something
	}
}
```

## <mark style="background: #FFB86CA6;">Pagination</mark>

책의 페이지처럼 데이터를 나눠서 제공하는 것

## <mark style="background: #FFF3A3A6;">Paging Property</mark>

pagination 기법을 쓰기 위해서 백엔드에서 임의로 만든 property

## <mark style="background: #BBFABBA6;">구조</mark>

![[Pasted image 20240411112107.png]]

| hasNext          | count    |
| ---------------- | -------- |
| 다음 데이터가 존재하는지 확인 | 총 데이터 개수 |

## <mark style="background: #BBFABBA6;">hasNext 활용</mark>

```jsx
{hasNext && <button onClick={handleLoadMore}>
	더 보기
</button>}
```

hasNext가 다음 데이터가 없으면 false인 것을 이용해서 버튼을 동적으로 보여 줄 수 있다.

## <mark style="background: #ABF7F7A6;">conditional rendering</mark>

## <mark style="background: #ADCCFFA6;">&&</mark>

```jsx
<div>
  <button onClick={handleClick}>토글</button>
  {show && <p>보인다 👀</p>}
</div>;
```

## <mark style="background: #ADCCFFA6;">||</mark>

```jsx
<div>
  <button onClick={handleClick}>토글</button>
  {hide || <p>보인다 👀</p>}
</div>;
```

## <mark style="background: #ADCCFFA6;">ternary</mark>

```jsx
<div>
  <button onClick={handleClick}>토글</button>
  {toggle ? <p>✅</p> : <p>❎</p>}
</div>;
```

## <mark style="background: #ADCCFFA6;">rendering 하지 않는 value</mark>

```jsx
const nullValue = null; 
const undefinedValue = undefined; 
const trueValue = true; 
const falseValue = false; 
const emptyString = ''; 
const emptyArray = [];
```

## <mark style="background: #ADCCFFA6;">주의할 rendering하는 value</mark>

```jsx
[num,setNum]=useState(0);
// ...
<div> 
	<button onClick={handleClick}>더하기</button> 
	{num && <p>num이 0 보다 크다!</p>} 
</div>
```

위  코드의 문제점은 num이 rendering되어서 화면에 보이게 된다는 것이다.
true, false는 rendering되지 않으니 다음과 같이 바꿔야 한다.

```jsx
[num,setNum]=useState(0);
// ...
<div> 
	<button onClick={handleClick}>더하기</button> 
	{(num>0) && <p>num이 0 보다 크다!</p>} 
</div>
```
## <mark style="background: #FFF3A3A6;">offset 기반 pagination</mark>

원래 것과의 차이,거리 (지금까지 받은 데이터 개수)

```http
GET https://example.com/posts?offset=20&&limit=6
6개씩, 현재 20개 데이터 받음.
```

![[Pasted image 20240411110207.png]]

하지만 위의 그림처럼 원래 데이터가 변경되면 offset기반이 문제가 될 수 있다.

이를 해결하기 위해 cursor 기반 pagination이 생기게 된다.

## <mark style="background: #BBFABBA6;">ex) offset 기반</mark>

```jsx
export async function getReviews({
  order = "createdAt",
  offset = 0,
  limit = 6,
}) {
  const query = `order=${order}`;
  const response = await fetch(
    `https://learn.codeit.kr/api/film-reviews?${query}&&offset=${offset}&&limit=${limit}`
  );
  
  const body = await response.json();
  console.log(body);
  return body;
}
```

## <mark style="background: #FFF3A3A6;">cursor 기반 pagination</mark>

데이터를 가리키는 커서 기준

```http
GET https://example.com/posts?
```

![[Pasted image 20240411110401.png]]

위처럼 커서는 데이터가 변경되어도 해당 데이터를 지정하고 있기 때문에 이를 해결할 수 있다.

하지만 만들기 까다롭다는 단점이 있다.

## <mark style="background: #BBFABBA6;">ex) cursor 기반</mark>

```jsx
const [cursor, setCursor] = useState(null);
//...
const handleLoad = async (options) => {
	const { foods, paging: { nextCursor }, 
	} = await getFoods(options); // cursor를 사용하는 url을 fetch에 담아 데이터 받음.
	if (!options.cursor) {
		setItems(foods); 
	} else {
		setItems((prevItems) => [...prevItems, ...foods]); 
	} 
	setCursor(nextCursor); 
};
//...
// 처음 화면, order 변경시 랜더링
 useEffect(() => { 
    handleLoad({order});
  }, [order]);
//...
<div>
	<button onClick={handleNewestClick}>최신순</button>
	<button onClick={handleCalorieClick}>칼로리순</button>
	<FoodList items={sortedItems} onDelete={handleDelete} />
	{!!cursor && <button onClick={handleLoadMore}>더보기</button>}
</div>
```

## <mark style="background: #FF5582A6;">네트워크 로딩 처리</mark>

state가 변경될 때마다 페이지를 렌더링한다는 특징을 이용해서 쉽게 네트워크 로딩 처리를 할 수 있다.

1. 로딩을 처리하는 state를 만든다.
```jsx
  const [isLoading, setIsLoading] = useState(false);
```
2. 버튼에 연결한다. 조건 연산자로 버튼을 없애버려도 되고 아니면 disabled로 버튼을 비활성화해도 된다.
```jsx
//{isLoading && ( 조건 연산자
	<button disabled={isLoading} onClick={handleLoadMore}> //disabled
	  더 보기
	</button>
//)}
```
3. 데이터를 받아오기전엔 로딩 state를 true로 만든다.
```jsx
try {
      setIsLoading(true);
      result = await getReviews(options);
    }
```
4. 에러든 뭐든 통과가 되면 finally로 돌린다.
```jsx
finally {
	setIsLoading(false);
}
```

## <mark style="background: #FF5582A6;">네트워크 에러 처리</mark>

try catch 문에서 error를 state에 담고 그걸로 html을 만들어서 에러문을 처리하는 기법

```jsx
try{
	setLoadingError(null);
}catch(error){
	setLoadingError(error);
}
//...
{loadingError?.message && <span>{loadingError.message}</span>}
```

## <mark style="background: #FF5582A6;">Search 기법</mark>

search버튼을 눌렀을 때 해당되는 데이터를 가져와서 보여주는 방법

1. search를 받을 form 생성, 기본 동작을 지울 함수도 필요함.
```jsx
<form onSubmit={handleSearchSubmit}>
	<input name="search" />
	<button type="submit">검색</button>
</form>
```
2. search state 생성
```jsx
const [search, setSearch] = useState('');
```
3. 기본 동작을 지우고 state를 업데이트하는 함수
```jsx
  const handleSearchSubmit = (e) => {
    e.preventDefault();
    setSearch(e.target['search'].value);
  };
```
4. search를 담은 api 호출
```jsx
// api.jsx
export async function getFoods({
  order = '',
  cursor = '',
  limit = 10,
  search = '',
}) {
  const query = `order=${order}&cursor=${cursor}&limit=${limit}&search=${search}`;
  const response = await fetch(`https://learn.codeit.kr/api/foods?${query}`);
  if (!response.ok) {
    throw new Error('데이터를 불러오는데 실패했습니다');
  }
  const body = await response.json();
  return body;
}
```
5. useEffect 처리
```jsx
  useEffect(() => {
    handleLoad({
      order,
      search,
    });
  }, [order,search]);
```

search state가 바뀌면 useEffect를 통해 감지되어 api 호출을 다시 보내서 데이터를 받은 후 item이 바뀜.

## <mark style="background: #FF5582A6;">React Form</mark>

React에서 Form을 만들 수 있다. 정확히는 Form을 담고 있는 Component로 제작한다.

state가 중요한 역할을 한다.

```jsx
function ReviewForm() {
  const [title, setTitle] = useState("");
  const [rating, setRating] = useState("");
  const [content, setContent] = useState("");
  const handleTitleChange = (e) => {
    setTitle(e.target.value);
  };
  const handleRatingChange = (e) => {
    const nextRating = +e.target.value || 0;
    setRating(nextRating);
  };
  const handleContentChange = (e) => {
    setContent(e.target.value);
  };

  return (
    <form className="ReviewForm">
      <input type="text" value={title} onChange={handleTitleChange} />
      <input type="text" value={rating} onChange={handleRatingChange} />
      <textarea value={content} onChange={handleContentChange}></textarea>
    </form>
  );
}
```

## <mark style="background: #FFB86CA6;">value</mark>

html과 같이 input안의 값을 의미한다.

value에 state를 넣고 onChange로 처리해주면 change가 일어날때마다 랜더링되어 실시간으로 화

면에 반영된다.

## <mark style="background: #FFB86CA6;">onChange</mark>

html과는 다르게 input 처럼 동작하게 된다. html에서는 전부 입력하고 나왔을 때 change가 일어나

는데 react에서는 한글자 한글자마다 event가 일어나게 된다.

## <mark style="background: #FFB86CA6;">onSumbit</mark>

Form을 제출할 때 발동되는 prop, callback을 담을 수 있다.

주로 preventDefault를 이용해서 기본 동작을 차단하고 원하는 함수를 실행하는데에 사용하는 React의 eventHandler

## <mark style="background: #FFB86CA6;">Form에서 state 한번에 관리하는 기법</mark>

input, textarea에는 name과 value를 prop으로 넣을 수 있다.

value에 값을 넣으면 화면에 value의 값이 표시되게 된다.

이를 통해서 state에 객체를 형성하고 `[name]:value` 형식으로 각각 저장하면 한번에 

관리할 수 있게 된다.


1. state에 객체를 넣고 한번에 처리
```jsx
  const [values, setValues] = useState({
    title: "",
    rating: 0,
    content: "",
  });
```

2. return 하는 input들
```jsx
<input
	name="title"
	type="text"
	value={values.title}
	onChange={handleChange}
/>
<input
	name="rating"
	type="number"
	value={values.rating}
	onChange={handleChange}
/>
<textarea
	name="content"
	value={values.content}
	onChange={handleChange}
></textarea>
```

3. 모든 state를 포함하는 handler 추가
```jsx
  const handleChange = (e) => {
    const { name, value, type } = e.target;
    setValues((prevValues) => ({
      ...prevValues,
      [name]: sanitize(type,value),
      // computted key( [] )로 써야 name 변수가 내려올 수 있다.
    }));
  };
```

4. type에 따라서 string 처리
```jsx
function sanitize(type, value) { 
	switch (type) { 
		case 'number': 
			return Number(value) || 0; 
		
		default: 
			return value; 
	} 
}
```

## <mark style="background: #FF5582A6;">Controlled Component</mark>

input을 입력하는거 자체를 react에서 제어하는 것

## ex)
```jsx
const handleChange = (e) => {
    const { name, value, type } = e.target;
    setValues((prevValues) => ({
      ...prevValues,
      [name]:
        typeof sanitize(type, value) === "string"
          ? sanitize(type, value).toUpperCase()
          : sanitize(type, value), // computted key( [] )로 써야 name 변수가 내려올 수 있다.
    }));
  };
//...

return(
<form className="ReviewForm" onSubmit={handleSubmitClick}>
      <input
        name="title"
        type="text"
        value={values.title}
        onChange={handleChange}
      />
      
      <input
        name="rating"
        type="number"
        value={values.rating}
        onChange={handleChange}
      />

      <textarea
        name="content"
        value={values.content}
        onChange={handleChange}
      ></textarea>

      <button type="submit">확인</button>

</form>
)
```

위 코드대로면 input에 소문자를 넣어도 자동으로 upperCase로 넣어지게 된다.

해당하는 state와 value가 같아진다 > prop과 실제 보이는 화면이 같아지기 때문에 코드 짤때 수월하다.

## <mark style="background: #FF5582A6;">File Input</mark>

보안으로 인해서 file input은 파일의 경로를 C:\fakepath\ 등의 가상 경로로 지정하기 때문에 file input은 반드시 비제어 방식으로 해야한다.

value 속성은 사용자만 바꿀 수 있고 JS로 바꿀 때는 빈 문자열로만 바꿀 수 있다.

```jsx
// 이상태에서 state와 onChange를 이용해서 value를 저장한다.
return <input type="file" onChange={handleChange} />;

// App.jsx
const [values, setValues] = useState({
	title: "",
	rating: 0,
	content: "",
	imgFile: null,
});
  
const handleChange = (name, value, type) => {
setValues((prevValues) => ({
  ...prevValues,
  [name]:
	typeof sanitize(type, value) === "string"
	  ? sanitize(type, value).toUpperCase()
	  : sanitize(type, value), // computted key( [] )로 써야 name 변수가 내려올 수 있다.
}));
};

<FileInput
	name="imgFile"
	value={values.imgFile}
	onChange={handleChange}
/>

// FileInput.jsx
export default function FileInput({ name, onChange }) {
  const handleChange = (e) => {
    onChange(name, e.target.files[0], e.target.type);
  };
  return <input type="file" onChange={handleChange} />;
}
```

## <mark style="background: #FF5582A6;">useRef</mark>

실제 렌더링되는 Element 자체를 참조한다.

```jsx
const inputRef = useRef();
```

```jsx
<input
	name="title"
	type="text"
	value={values.title}
	onChange={handleInputChange}
	ref={inputRef}
/>
```

![[Pasted image 20240415115755.png]]

## <mark style="background: #FF5582A6;">파일 초기화</mark>

```jsx
// inputRef
const inputRef = useRef();

//handler
  const handlerClearClick = () => {
    const inputNode = inputRef.current;
    console.log(inputNode);
    if (!inputNode) return;
    inputNode.value = "";
    onChange(name, null);
  };

// return
<>
  <input type="file" onChange={handleChange} ref={inputRef} />;
  {value && <button onClick={handlerClearClick}>X</button>}
</>
```

제어 Component로 file input을 사용할 수 없기 때문에 inputRef를 이용해서 참조한 후 그 데이터의 값을 변경하는 식으로 작동한다.

## <mark style="background: #FF5582A6;">파일 미리 보기</mark>

파일 객체를 ObjectURL로 만들면 로컬 호스트에 파일이 저장되며 해당 URL을 사용할 수 있다.

이를 이용해서 파일을 선택하면 이미지가 나오도록 해보자.

```jsx
export default function FileInput({ name, value, onChange }) {
// 참조로 input 가져와서
  const inputRef = useRef();
  // 미리보기 state
  const [preview, setPreview] = useState();
  // 상위 Component에 있는 값 수정
  const handleChange = (e) => {
    onChange(name, e.target.files[0], e.target.type);
  };
  // input value를 clear하는 함수, state도 같이 해줘야 한다.
  const handlerClearClick = () => {
    const inputNode = inputRef.current;
    console.log(inputNode);
    
    if (!inputNode) return;
    
    inputNode.value = "";
    onChange(name, null);
  };

// value를 감시해서 바뀌면 preview를 바꿈.
  useEffect(() => {
    if (!value) return;  
    const nextPreview = URL.createObjectURL(value);
    setPreview(nextPreview);
  }, [value]);
  
  return (
    <>
      <img src={preview} alt="이미지 미리보기" />
      <input type="file" accept="image/png, image/jpeg" onChange={handleChange} ref={inputRef} />
      {value && <button onClick={handlerClearClick}>X</button>}
    </>
  );
}
```

ObjectURL은 만들 때마다 웹 브라우저에 메모리를 할당한다. 이는 외부의 상태를 바꾸는 것인데 이를 `sideEffect` 라고 한다.

React에서는 network request나 메모리 할당 처럼 외부의 상태를 바꿀 때 useEffect를 사용한다.

## <mark style="background: #FFB86CA6;">cleanup 함수</mark>

위 코드에는 사실 한가지 디테일이 필요하다.

useEffect의 return값으로 함수를 주게 되면 다음 effect가 발생했을 때 자동으로 먼저 실행되게 된

다. 

이를 통해서 preview가 바뀔 때 이전 메모리를 삭제하는 코드를 작성하면 최적화가 될 수 있다.

여기서 return으로 주는 함수를 cleanup function이라고 한다.

```jsx
useEffect(() => {
  if (!value) return;  
  const nextPreview = URL.createObjectURL(value);
  setPreview(nextPreview);
  return ()=>{
    setPreview();
    URL.revokeObjectURL(nextPreview);
  }
}, [value]);
```

return하는 저 함수는 컴포넌트가 사라지기 직전에, 다음 콜백이 실행됐을 때 실행된다.

## <mark style="background: #FFF3A3A6;">ex) Timer</mark>

```jsx
import { useEffect, useState } from 'react';

function Timer() {
  const [second, setSecond] = useState(0);

  useEffect(() => {
    const timerId = setInterval(() => {
      console.log('타이머 실행중 ... ');
      setSecond((prevSecond) => prevSecond + 1);
    }, 1000);
    console.log('타이머 시작 🏁');

    return () => {
      clearInterval(timerId);
      console.log('타이머 멈춤 ✋');
    };
  }, []);

  return <div>{second}</div>;
}

function App() {
  const [show, setShow] = useState(false);

  const handleShowClick = () => setShow(true);
  const handleHideClick = () => setShow(false);

  return (
    <div>
      {show && <Timer />}
      <button onClick={handleShowClick}>보이기</button>
      <button onClick={handleHideClick}>감추기</button>
    </div>
  );
}

export default App;
```

button을 눌러서 보이거나 감추면 화면이 재렌더링되기 때문에 타이머가 처음부터 돌아가게 되고 결과적을로 감췄다가 보이게 하면 초기화된다.

## <mark style="background: #FF5582A6;">Side Effect</mark>

외부의 값이나 상태를 변경하는 등 외부에 부수적인 작용을 하는 것을 의미한다.

```jsx
let count = 0;

function add(a, b) {
  const result = a + b;
  count += 1; // 함수 외부의 값을 변경
  return result;
}

const val1 = add(1, 2);
const val2 = add(-4, 5);
```

외부의 값을 변경하기 때문에 위의 코드는 Side Effect가 존재한다고 본다.

또다른 예시로는 `console.log`처럼 외부 웹 브라우저의 상태를 변경하는 것이 있다.

`useEffect`는 Component함수 안에서 Side Effect를 실행할 때 사용한다.
(DOM변경, 브라우저에 데이터 저장, Network Request)(React 외부에 있는 데이터나 상태를 변경)

## <mark style="background: #FFB86CA6;">ex) 페이지 정보 변경</mark>

```jsx
useEffect(() => {
  document.title = title; // 페이지 데이터를 변경
}, [title]);
```

## <mark style="background: #FFB86CA6;">ex) network request</mark>

```jsx
useEffect(() => {
  fetch('https://example.com/data') // 외부로 네트워크 리퀘스트
    .then((response) => response.json())
    .then((body) => setData(body));
}, [])
```

## <mark style="background: #FFB86CA6;">ex) 데이터 저장</mark>

```jsx
useEffect(() => {
  localStorage.setItem('theme', theme); // 로컬 스토리지에 테마 정보를 저장
}, [theme]);
```

## <mark style="background: #FFB86CA6;">ex) 타이머</mark>

```jsx
useEffect(() => {
  const timerId = setInterval(() => {
    setSecond((prevSecond) => prevSecond + 1);
  }, 1000); // 1초마다 콜백 함수를 실행하는 타이머 시작
  
  return () => {
    clearInterval(timerId);
  }
}, []);
```

## <mark style="background: #FFB86CA6;">useEffect를 이용해 sideEffect를 처리하면 좋은 경우</mark>

동기화 (컴포넌트 안에 데이터와 리액트 바깥에 있는 데이터를 일치) 에 좋다.

## <mark style="background: #FFF3A3A6;">ex) 이벤트 핸들러만 사용할 경우</mark>

```jsx
import { useState } from 'react'; 

const INITIAL_TITLE = 'Untitled'; 

function App() { 
	const [title, setTitle] = useState(INITIAL_TITLE); 
	
	const handleChange = (e) => { 
		const nextTitle = e.target.value; 
		setTitle(nextTitle); 
		document.title = nextTitle; 
	}; 
	
	const handleClearClick = () => { 
		const nextTitle = INITIAL_TITLE; 
		setTitle(nextTitle); 
		document.title = nextTitle; 
	}; 
	return (
		<div> 
			<input value={title} onChange={handleChange} /> 
			<button onClick={handleClearClick}>초기화</button> 
		</div> 
	); 
} 

export default App;
```

`setTitle` 을 설정할 때마다 document.title을 따로 실행해서 title을 변경해야 하는게 귀찮다.

## <mark style="background: #FFF3A3A6;">ex) useEffect로 Side Effect 처리</mark>

```jsx
import { useEffect, useState } from 'react';

const INITIAL_TITLE = 'Untitled';

function App() {
  const [title, setTitle] = useState(INITIAL_TITLE);

  const handleChange = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
  };

  const handleClearClick = () => {
    setTitle(INITIAL_TITLE);
  };

  useEffect(() => { // title이 바뀔때마다 side effect를 처리한다.
    document.title = title;
  }, [title]);

  return (
    <div>
      <input value={title} onChange={handleChange} />
      <button onClick={handleClearClick}>초기화</button>
    </div>
  );
}

export default App;
```

## <mark style="background: #FF5582A6;">별점 처리</mark>

```jsx
import "./Rating.css";

const RATINGS = [1, 2, 3, 4, 5];
  
const Star = ({ selected = false }) => {
  const className = `Rating-star ${selected ? "selected" : ""}`;
  return <span className={className}>★</span>;
};
  
export default function Rating({ value = 0 }) {
  return (
    <div>
      {RATINGS.map((rating, index) => (
        <Star selected={value >= rating} key={index} />
      ))}
    </div>
  );
}
```

rating에 따라서 별이 selected일지 ""일지 결정하는 로직

## <mark style="background: #FFB86CA6;">별점 처리 심화</mark>

자신이 별점을 처리할 수 있게 해보자.
```jsx
// 하나의 Component
<RatingInput
	name="rating"
	type="number"
	value={values.rating}
	onChange={handleChange}
/>

// 각 star를 감쌀 커스텀 input
import { useState } from "react";
import Rating from "./Rating";

export default function RatingInput({name, value, onChange}) {
// input에서 rating과
  const [rating, setRating] = useState(value);
  
 // input만의 핸들러들을 특정한다.
  const handleSelect = (nextValue) => onChange(name, nextValue);
  const handleMouseOut = () => setRating(value);

  return (
    <Rating
      value={rating}
      onSelect={handleSelect}
      onHover={setRating}
      onMouseOut={handleMouseOut}
    />
  );
}

// input에서 받은 함수들을 연결하는 Component와 내부의 star Component
import "./Rating.css";

const RATINGS = [1, 2, 3, 4, 5];

const Star = ({ selected = false, rating, onSelect, onHover }) => {
  const className = `Rating-star ${selected ? "selected" : ""}`;
  const handleClick = onSelect ? () => onSelect(rating) : undefined;
  const handleMouseOver = onHover ? () => onHover(rating) : undefined;
  
  return (
    <span
      className={className}
      onClick={handleClick}
      onMouseOver={handleMouseOver}
    >
      ★
    </span>
  );
};

export default function Rating({ value = 0, onSelect, onHover, onMouseOut }) {
  return (
    <div onMouseOut={onMouseOut}>
      {RATINGS.map((rating) => (
        <Star
          selected={value >= rating}
          key={rating}
          rating={rating}
          onSelect={onSelect}
          onHover={onHover}
        />
      ))}
    </div>
  );
}
```

## <mark style="background: #FF5582A6;">Post 요청 보내기</mark>

```jsx
export async function createReviews(formData) {

  const response = await fetch(`${BASEURL}/film-reviews`, {
    method: "POST",
    body: formData,
  });

  if (!response.ok) {
    throw new Error("리뷰 실패");
  }
  
  const body = await response.json();
  return body;
}
```

```jsx
// onSubmit에 넣을 콜백함수 api.jsx
const handleSubmit = async (e) => {
	e.preventDefault();
	const formData = new FormData();
	formData.append("title", values.title);
	formData.append("rating", values.rating);
	formData.append("content", values.content);
	formData.append("imgFile", values.imgFile);
	try {
	  setIsSubmitting(true);
	  setSubmittingError(null);
	  await createReviews(formData);
	} catch (error) {
	  setSubmittingError(error);
	} finally {
	  setIsSubmitting(false);
	}
	setValues(BASEVALUES);
};
```

formData라는 형태를 이용해서 객체를 생성하고 그걸 URL에 담아서 POST 요청을 보낸다.

이때 POST에 response 데이터가 존재하면 그것을 이용해서 아이템 리스트 실시간 반영 같은 동작을 구현할 수 있다.

## <mark style="background: #FF5582A6;">글 삭제, 수정 로직</mark>

## <mark style="background: #FFB86CA6;">삭제</mark>
## <mark style="background: #FFF3A3A6;">삭제 클릭시 전체 데이터 중 삭제 클릭한 것만 빼고 랜더링</mark>
```jsx
const handleDelete = (id) => {
    const nextItems = items.filter((item) => item.id !== id);
    setItems(nextItems);
  };
 
//...
      
<ReviewList items={items} onDelete={handleDelete} />
```

## <mark style="background: #FFF3A3A6;">서버 데이터까지 삭제</mark>

## <mark style="background: #BBFABBA6;">DELETE fetch 생성</mark>

```jsx
export async function deleteReviews(id) {
  const response = await fetch(`${BASEURL}/film-reviews/${id}`, {
    method: "DELETE",
  });
  
  if (!response.ok) {
    throw new Error("리뷰 삭제 실패");
  }
  
  const body = await response.json();
  return body;
}
```

## <mark style="background: #BBFABBA6;">기존 onDelete에 연결</mark>

```jsx
const handleDelete = async (id) => {
    const result = await deleteReviews(id);
    if (!result) return;
    setItems((prevItems) => prevItems.filter((item) => item.id !== id)); // 앞에가 비동기니깐
 };
```

## <mark style="background: #FFB86CA6;">수정</mark>
## <mark style="background: #FFF3A3A6;">편집 취소 기능과 편집 모드(editingId === item.id) 되면 다른 Component를 사용하는 로직, ReviewList</mark>

```jsx
const ReviewList = ({ items, onDelete }) => {
  const [editingId, setEditingId] = useState(null);
  // 취소 버튼 누르면 editing 해제
  const handleCancel = () => setEditingId(null);
  
  return (
    <ul>
      {items.map((item) => {
        if (item.id === editingId) {
          const { imgUrl, title, rating, content } = item;
          const initialValues = { title, rating, content };
          return (
            <li key={item.id}>
              <ReviewForm
                item={item}
                onDelete={onDelete}
                initialValues={initialValues}
                initialPreview={imgUrl}
                onCancel={handleCancel}
              />
            </li>
          );
        }
        return (
          <li key={item.id}>
            <ReviewListItem
              item={item}
              onDelete={onDelete}
              onEdit={setEditingId}
            />
          </li>
        );
      })}
    </ul>
  );
};
```

## <mark style="background: #FFF3A3A6;">편집 아닐시 내부</mark>

받은 setEditingId 에 item.id를 찍고 Delete도 삭제를 그대로 받아서 item.id를 찍는 모습

```jsx
const ReviewListItem = ({ item, onDelete, onEdit }) => {
  const handleDeleteClick = () => {
    onDelete(item.id);
  };
  
  const handleEditClick = () => {
    onEdit(item.id);
  };
  
  return (
    <div className="ReviewListItem">
      <img className="ReviewListItem-img" src={item.imgUrl} alt={item.title} />
      <div>
        <h1>{item.title}</h1>
        <Rating value={item.rating} />
        <p>{formatDate(item.createdAt)}</p>
        <p>{item.content}</p>
        <button onClick={handleDeleteClick}>삭제</button>
        <button onClick={handleEditClick}>수정</button>
      </div>
    </div>
  );
};
```

## <mark style="background: #FFF3A3A6;">편집시 내부</mark>

부모 컴포넌트에서 받은 함수(setEditingId(null))을 cancel button에 추가함. (onCancel prop)

Reviewform을 수정 누른 아이템으로 보여주기 위해서 기본값을 설정하고(initialValuess=BASEVALUES) 수정하기 위

해서 부른 ReviewForm은 데이터가 담긴 채로 들어가서 해당 아이템을 보여주고 아닌 경우 기본 값(BASEVALUES)이 

들어간다.

수정시 이미 정해진 그림을 보여주고 파일 선택도 해야되기에 FileInput Component로 prop 형식으로 넘긴다. (initialPreview)

```jsx
function ReviewForm({
  initialPreview,
  initialValues = BASEVALUES,
  onSubmitSuccess,
  onCancel,
}) {
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submittingError, setSubmittingError] = useState(false);
  const inputRef = useRef();
  const [values, setValues] = useState(initialValues);
  
  const handleChange = (name, value) => {
    setValues((prevValues) => ({
      ...prevValues,
      [name]: value,
    }));
  };
  
  const handleInputChange = (e) => {
    const { name, value } = e.target;
    handleChange(name, value);
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    const formData = new FormData();
    formData.append("title", values.title);
    formData.append("rating", values.rating);
    formData.append("content", values.content);
    formData.append("imgFile", values.imgFile);
    let result;
    try {
      setIsSubmitting(true);
      setSubmittingError(null);
      result = await createReviews(formData);
    } catch (error) {
      setSubmittingError(error);
    } finally {
      setIsSubmitting(false);
    }
    const { review } = result;
    onSubmitSuccess(review);
    setValues(BASEVALUES);
  };
 return (
    <form className="ReviewForm" onSubmit={handleSubmit}>
      <FileInput
        name="imgFile"
        value={values.imgFile}
        initialPreview={initialPreview}
        onChange={handleChange}
      />
      <h1>{values.title}</h1>
      <input
        name="title"
        type="text"
        value={values.title}
        onChange={handleInputChange}
        ref={inputRef}
      />
      <RatingInput
        name="rating"
        value={values.rating}
        onChange={handleChange}
      />
      <textarea
        name="content"
        value={values.content}
        onChange={handleInputChange}
      ></textarea>
      <button type="submit" disabled={isSubmitting}>
        확인
      </button>
      {onCancel && <button onClick={onCancel}>취소</button>}
      {submittingError?.message && <span>submittingError.message</span>}
    </form>

  );
```

## <mark style="background: #BBFABBA6;">File Input</mark>

useRef()를 이용해서 input의 value를 가져오고 (useRefInstance.current.file[각 인덱스]를 ObjectURL 방식으로 새 주소에 할당해서 가져와야 한다.) 해당 value를 preview에 넣은 후 preview를 

img에 쏴주는 방식이다.

preview에 기본값인 initialPreview를 받아서 처음이면 undefined를 수정을 누른 거면 부모에서 내려받은 imgUrl이 

해당 preview가 되게 한다.

이렇게 해야 파일 선택을 또 했을 때 변경도 되면서 초기에 기존 파일이 들어오게 할 수 있다.

```jsx
export default function FileInput({ name, value, onChange, initialPreview }) {
  const inputRef = useRef();
  const [preview, setPreview] = useState(initialPreview);
  const handleChange = (e) => {
    onChange(name, e.target.files[0], e.target.type);
  };
  
  const handlerClearClick = () => {
    const inputNode = inputRef.current;
    if (!inputNode) return;
  
    inputNode.value = "";
    onChange(name, null);
  };
  
  useEffect(() => {
    if (!value) return;
  
    const nextPreview = URL.createObjectURL(value);
    setPreview(nextPreview);
  
    return () => {
      setPreview(initialPreview);
      URL.revokeObjectURL(nextPreview);
    };
  }, [value, initialPreview]);
  
  return (
    <>
      <img src={preview} alt="이미지 미리보기" />
      <input type="file" onChange={handleChange} ref={inputRef} />
      {value && <button onClick={handlerClearClick}>X</button>}
    </>
  );
}
```

## <mark style="background: #FFF3A3A6;">서버 데이터까지 수정하기</mark>

```jsx
// put요청을 보내는 fetch 생성
export async function updateReviews(id, formData) {
  const response = await fetch(`${BASEURL}/film-reviews/${id}`, {
    method: "PUT",
    body: formData,
  });
  if (!response.ok) {
    throw new Error("리뷰 수정 실패");
  }
  const body = await response.json();
  return body;
}
```

formData을 PUT 요청으로 보내서 수정하는 fetch

```jsx
// 수정 성공시 item에 실시간 반영
const handleUpdateSuccess = (review) => {
	setItems((prevItems) => {
	  const splitIdx = prevItems.findIndex((item) => item.id === review.id);
	  return [
		...prevItems.slice(0, splitIdx),
		review,
		...prevItems.slice(splitIdx + 1),
	  ];
	});
};

//...

<ReviewList
	items={items}
	onDelete={handleDelete}
	onUpdate={updateReviews} // 서버 데이터 수정
	onUpdateSuccess={handleUpdateSuccess} // 데이터 바꾸고 실시간 반영
/>
```

## <mark style="background: #BBFABBA6;">ReviewList</mark>

```jsx
const ReviewList = ({ items, onDelete, onUpdate, onUpdateSuccess }) => {
  const [editingId, setEditingId] = useState(null);
  const handleCancel = () => setEditingId(null);

  return (
    <ul>
      {items.map((item) => {
        if (item.id === editingId) {
          // item에서 id, imgUrl 등등 데이터를 꺼냄
          const { id, imgUrl, title, rating, content } = item;
          const initialValues = { title, rating, content };
  
          const handleSubmit = (formData) => onUpdate(id, formData); // 서버 데이터 수정
  
          const handleSubmitSuccess = (review) => { // review를 받으면
            onUpdateSuccess(review); // 데이터 실시간 반영 후
            setEditingId(null); // 편집 모드를 종료
          };
  
          return (
            <li key={item.id}>
              <ReviewForm
                item={item}
                onDelete={onDelete}
                initialValues={initialValues}
                initialPreview={imgUrl}
                onCancel={handleCancel}
                onSubmit={handleSubmit} 
                // submit event에 넣기 위해서 내려보냄.
                // 추가와 같은 핸들러를 사용해서 보내는데 부모 컴포넌트에 의해서 수정하는 함수가 된것
                onSubmitSuccess={handleSubmitSuccess}
              />
            </li>
          );
        }
        return (
          <li key={item.id}>
            <ReviewListItem
              item={item}
              onDelete={onDelete}
              onEdit={setEditingId}
            />
          </li>
        );
      })}
    </ul>
  )

};
```

## <mark style="background: #BBFABBA6;">ReviewForm</mark>

```jsx
function ReviewForm({
  initialPreview,
  initialValues = BASEVALUES,
  onSubmitSuccess,
  onSubmit,
  onCancel,
}) {
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submittingError, setSubmittingError] = useState(false);
  const inputRef = useRef();
  const [values, setValues] = useState(initialValues);

  const handleChange = (name, value) => {
    setValues((prevValues) => ({
      ...prevValues,
      [name]: value,
    }));
  };

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    handleChange(name, value);
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    const formData = new FormData();
    formData.append("title", values.title);
    formData.append("rating", values.rating);
    formData.append("content", values.content);
    formData.append("imgFile", values.imgFile);
    let result;
    try {
      setIsSubmitting(true);
      setSubmittingError(null);
      result = await onSubmit(formData); // 수정후 result에 response를 담는다.
    } catch (error) {
      setSubmittingError(error);
    } finally {
      setIsSubmitting(false);
    }
    const { review } = result;
    onSubmitSuccess(review); // 수정을 성공하면 실시간으로 반영한다.
    setValues(BASEVALUES);
  };

  return (
    <form className="ReviewForm" onSubmit={handleSubmit}>
      <FileInput
        name="imgFile"
        value={values.imgFile}
        initialPreview={initialPreview}
        onChange={handleChange}
      />
      <h1>{values.title}</h1>
      <input
        name="title"
        type="text"
        value={values.title}
        onChange={handleInputChange}
        ref={inputRef}
      />
      <RatingInput
        name="rating"
        value={values.rating}
        onChange={handleChange}
      />
      <textarea
        name="content"
        value={values.content}
        onChange={handleInputChange}
      ></textarea>
      <button type="submit" disabled={isSubmitting}>
        확인
      </button>
      {onCancel && <button onClick={onCancel}>취소</button>}
      {submittingError?.message && <span>submittingError.message</span>}
    </form>
  );
}
```

서버에 담고 실시간 반영, 그리고 기타 loading이나 error처리까지 끝내면 서버 데이터 수정 완료이다.

## <mark style="background: #FF5582A6;">React Hook</mark>

use로 시작하는 함수를 React에서 `Hook`이라고 한다.

내가 작성한 코드를 다른 프로그램에 연결해서 그 값이나 기능을 사용하는 것
ex) useEffect, useState, useRef ...

## <mark style="background: #FFB86CA6;">규칙</mark>

1. Component 안에서 사용되어야 한다.
2. 함수의 최상위 스코프에서 사용되어야 한다.
	1. 반복문 안에서는 순서가 바뀔 수 있으며 React에서는 state를 순서로 판단하기 때문이다.

## <mark style="background: #FFB86CA6;">Custom Hook</mark>

사용자가 만든 Hook을 의미한다.

## <mark style="background: #FFF3A3A6;">ex) 비동기시 에러와 로딩을 처리하는 커스텀 훅</mark>

```jsx
import { useState } from "react";

function useAsync(asyncFunction) {
  const [pending, setPending] = useState(false);
  const [error, setError] = useState(null);
  const wrappedFunction = async (...args) => {
    try {
      setPending(true);
      setError(null);
      return await asyncFunction(...args);
    } catch (error) {
      setError(error);
      return;
    } finally {
      setPending(false);
    }
  };
  return [error, pending, wrappedFunction];
}
  
export default useAsync;
```

이렇게 훅을 만든 후 기존 state로 사용해서 각각 처리하던 방식에서 useAsync를 이용해서 한번에 받아올 수 있다.

```jsx
const [isLoading, loadingError, getReviwsAsync] = useAsync(getReviews);
//...
const handleLoad = async (options) => {
	let result = await getReviwsAsync(options);
    if (!result) return;
    const { reviews, paging } = result;
```

기존에 일일이 try catch 넣던거 보다 깔끔해졌다.

## <mark style="background: #FFF3A3A6;">ex) useToggle</mark>

```jsx
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  const toggle = () => setValue((prevValue) => !prevValue);
  return [value, toggle];
}
```

## <mark style="background: #FFF3A3A6;">ex) useTimer</mark>

```jsx
function useTimer(callback, timeout) {
  const [isRunning, setIsRunning] = useState(false);

  const start = () => setIsRunning(true);

  const stop = () => setIsRunning(false);

  useEffect(() => {
    if (!isRunning) return;

    const timerId = setInterval(callback, timeout); // 사이드 이펙트 발생
    return () => {
      clearInterval(timerId); // 사이드 이펙트 정리
    };
  }, [isRunning, callback, timeout]);
  
  return [start, stop];
}
```

## <mark style="background: #FFB86CA6;">useCallBack</mark>

함수를 계속 재렌더링하지 않고 고정시켜주는 훅, dependancy list가 바뀌지 않는 이상 함수를 다시 만들지 않고 재사용한다.

```jsx
const something = useCallback(callback,deps)
```

useEffect에 dependancy list에 의해서 무한 루프가 돌아갈 때 사용하면 루프가 멈춤.

setter함수 같은건 deps에 안 추가해도 되고 외부에서 참조하는 함수들을 deps에 추가해야한다.

## <mark style="background: #FFB86CA6;">exhaustive-deps 규칙</mark>

Prop이나 State와 관련된 값은 되도록이면 빠짐없이 디펜던시에 추가해서 항상 최신 값으로 `useEffect` 나 `useCallback` 을 사용하도록 권장

```jsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [num, setNum] = useState(0);

  const addCount = () => {
    setCount((c) => c + 1); // count를 늘리고
    console.log(`num: ${num}`); // num을 보여주는 함수
  };

  const addNum = () => setNum((n) => n + 1); // num을 늘리는 함수

  useEffect(() => {
    console.log('timer start');
    const timerId = setInterval(() => {
      addCount(); // addCount를 1초마다 실행
    }, 1000);

    return () => {
      clearInterval(timerId); // clean-up함, 초기화
      console.log('timer end');
    };
  }, [addCount]); // addCount가 재생성될때마다

  return (
    <div>
      <button onClick={addCount}>count: {count}</button>
      <button onClick={addNum}>num: {num}</button>
    </div>
  );
}

export default App;
```

addCount 안에 있는 num이 바뀔때마다 랜더링 되고 그 랜더링을 감시한다. 

하지만 여기서 무한 루프가 된다. addCount가 랜더링되면 감지되어 다시 안에서 랜더링 또 감지 랜더링...

이를 위해선 useCallback을 사용해서 제한해야 한다.

```jsx
import { useCallback, useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [num, setNum] = useState(0);

  const addCount = useCallback(() => {
    setCount((c) => c + 1);
    console.log(`num: ${num}`);
  }, [num]); // 이제 addCount는 num이 바뀔 때만 함수를 새로 만들고 나머지는 재사용한다.

  const addNum = () => setNum((n) => n + 1);

  useEffect(() => {
    console.log('timer start');
    const timerId = setInterval(() => {
      addCount();
    }, 1000);

    return () => {
      clearInterval(timerId);
      console.log('timer end');
    };
  }, [addCount]); // useCallback으로 인해 num이 바뀔 때만 실행되게 바뀜

  return (
    <div>
      <button onClick={addCount}>count: {count}</button>
      <button onClick={addNum}>num: {num}</button>
    </div>
  );
}

export default App;
```

## <mark style="background: #FFF3A3A6;">번외로 여기선 파라미터로 받아버리면 callback안쓰고 구현된다</mark>

```jsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [num, setNum] = useState(0);

  const addCount = (log) => { // arg로 받음.
    setCount((c) => c + 1);
    console.log(log);
  }

  const addNum = () => setNum((n) => n + 1);

  useEffect(() => {
    console.log('timer start');
    const timerId = setInterval(() => {
      addCount(`num ${num}`); // 파라미터로 감시되고 있는 num을 넣음.
    }, 1000);

    return () => {
      clearInterval(timerId);
      console.log('timer end');
    };
  }, [num]);

  return (
    <div>
      <button onClick={addCount}>count: {count}</button>
      <button onClick={addNum}>num: {num}</button>
    </div>
  );
}

export default App;
```

## <mark style="background: #FF5582A6;">Prop Drilling</mark>

드릴로 땅을 파듯이 상위 컴포넌트에서 하위 컴포넌트로 반복해서 Prop을 내려주는 상황

## <mark style="background: #FF5582A6;">Context</mark>

Prop Drilling 없이 전역적으로 특정 정보를 쓸 수 있는 기능

1. context가 affix인 파일 생성 (`localeContext`)
2. 파일 입력

```jsx
import { createContext } from "react";

const LocaleContext = createContext(defaultValue); // defaultValue는 없어도 됨

export default LocaleContext;
```

3. .Provider로 적어서 컴포넌트처럼 사용

```jsx
 return (
    <LocaleContext.Provider value="ko">
      <div>
        <button onClick={handleTotalView}>전체 보기</button>
        <button onClick={handleNewestClick}>최신순</button>
        <button onClick={handleBestClick}>베스트순</button>
      </div>
      <ReviewForm
        onSubmitSuccess={handleCreateSuccess}
        onSubmit={createReviews}
      />
      <ReviewList
        items={items}
        onDelete={handleDelete}
        onUpdate={updateReviews}
        onUpdateSuccess={handleUpdateSuccess}
      />
      {hasNext && (
        <button disabled={isLoading} onClick={handleLoadMore}>
          더 보기
        </button>
      )}
      {loadingError?.message && <span>{loadingError.message}</span>}
	</LocaleContext.Provider>
```

value에 넣어서 해당 value를 하위 컴포넌트로 내려줄 수 있다.

4. `useContext`를 이용해서 하위 컴포넌트에서 Context값을 사용할 수 있다.

```jsx
const locale = useContext(LocaleContext);
//...
<p>locale: {locale}</p>
```

## <mark style="background: #FFB86CA6;">Context 분리</mark>

Context를 쓸 때마다 prop으로 옮겨 넣고 이런것 말고 하위 컴포넌트 전역에서 다 쓸 수 있으면서 분리시키기

```jsx
// context를 사용하는 hook 만들기
import { createContext, useContext, useState } from "react";

const LocaleContext = createContext(); // context 생성(기본)

export function LocaleProvider({ defaultValue = "ko", children }) { // context가 적용된 컴포넌트로 하위 컴포넌트를 감싼다.
  const [locale, setLocale] = useState(defaultValue);
  return (
    <LocaleContext.Provider value={{ locale, setLocale }}>
      {children}
    </LocaleContext.Provider>
  );
}

export function useLocale() { // 외부에서 locale state를 사용할 수 있게 함.
  const context = useContext(LocaleContext);
  if (!context)
    throw new Error("반드시 LocaleProvider 안에서 사용해야 합니다.");
  return context.locale; // value에 객체 형태({locale, setLocale})로 넣었기 때문에 프로퍼티 형태로 꺼낸거임
}

export function useSetLocale() { // 외부에서 setLocale state를 사용할 수 있게 함.
  const context = useContext(LocaleContext);
  if (!context)
    throw new Error("반드시 LocaleProvider 안에서 사용해야 합니다.");
  return context.setLocale; // value에 객체 형태({locale, setLocale})로 넣었기 때문에 프로퍼티 형태로 꺼낸거임
}

export default LocaleContext;
```


위 코드는 Context를 입히는 (Context.Provider 이렇게 귀찮게 안하고) 컴포넌트를 하나 만들고 그 안에 state를 넣

어서 외부에서는 Provider 안에만 있다면 훅을 이용해서 해당 state에 접근할 수 있게 만든 것이다.

이렇게 하면 Provider를 재사용할 수 있다.

## <mark style="background: #FFF3A3A6;">다국어 기능 완성하기</mark>

dictonary 자료구조 사용한다.

```jsx
import { useLocale } from "../contexts/LocaleContext";

const dict = {
  ko: {
    "confirm button": "확인",
    "cancel button": "취소",
    "edit button": "수정",
    "delete button": "삭제",
  },
  en: {
    "confirm button": "OK",
    "cancel button": "Cancel",
    "edit button": "Edit",
    "delete button": "Delete",
  },
};

function useTranslate() {
  const locale = useLocale();
  const translate = (key) => dict[locale][key] || "";
  return translate;
}

export default useTranslate;
```

위의 useTranslate 훅은 key값을 받아서 번역해주는 함수를 리턴하기 때문에 이제 다른 컴포넌트에서 사용할때 key

값만 arg에 넣어주면 실행되게 된다.

```jsx
const t = useTranslate();
//...
<button onClick={handleDeleteClick}>{t("delete button")}</button>
```

## <mark style="background: #FF5582A6;">State Management</mark>

화면에서 사용하는 데이터를 관리하는 것

context의 등장으로 전역적으로 사용하는 정보라는 개념이 등장했지만 데이터의 변경을 어디서 하는지 알기 힘들어 졌다.

## <mark style="background: #FFB86CA6;">Flux</mark>

![[Pasted image 20240417121850.png]]
![[Pasted image 20240417121959.png]]

## <mark style="background: #FFB86CA6;">Redux</mark>

Flux보다 더 단순한 라이브러리
## <mark style="background: #FFB86CA6;">React Context</mark>

React에서 개발한 더 괜찮은 Context

## <mark style="background: #FFB86CA6;">Redux, React Context를 함께 사용하던 시기</mark>

Redux를 사용하다보니 단점 발견
1. 결국엔 모든 데이터를 Redux에 넣어버림.
2. 비동기 작업시 무의미한 코드를 작성해야했다.

## <mark style="background: #FFF3A3A6;">개발자들의 해결 과정</mark>

Client-state (코드잇의 실행기 실행 같은 것) , Server-state (강의 목록을 가져오는 것)를 나눴다.

FE는 Client-state에만 집중하고 나중에 서버와 동기화해볼까?

## <mark style="background: #BBFABBA6;">React Query, SWR</mark>

request를 적어놓으면 알아서 서버 관리를 한다.

## <mark style="background: #BBFABBA6;">Recoil</mark>

전역적으로 쓸 수 있는 useState

Context의 단점에는 
1. 데이터를 추가할 때마다 context가 하나씩 늘어남.
2. 데이터의 구조가 context의 구조와 섞인다.

그래서 Rocoil에서는 따로 빼서 관리함.
![[Pasted image 20240417122755.png]]

