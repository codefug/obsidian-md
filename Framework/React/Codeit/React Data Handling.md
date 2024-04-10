## Array Rendering

## map rendering

map으로 JSX를 리턴하면 JSX를 여러개 둔 것처럼 작동한다.

```jsx
function ComponentName({items}){
	return <ul>{items.map((item)=>return <li>{item.property}</li>)}</ul>
}
```


## ex)
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

## sort 활용

state를 이용해서 sort를 다양한 방식으로 바꿀 수 있다.

```jsx
const [order, setOrder] = useState("createdAt");
const sortedItems = items.sort((a,b)=> b[order] - a[order]);
```

onClick handler 같은 곳에 setOrder를 넣어서 order를 변경하게 하면 실시간으로 정렬의 종류를 변

경할 수 있다. (어떤 property를 기준으로 정렬하는지 변경할 수 있다.)

## filter 활용

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