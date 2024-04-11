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

## 네트워크 로딩 처리

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
    } catch (error) {
      console.error(error);
      return;
    }
```
4. 에러든 뭐든 통과가 되면 finally로 돌린다.
```jsx
finally {
	setIsLoading(false);
}
```

   