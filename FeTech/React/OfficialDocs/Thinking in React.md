리액트를 사용한다는 건 UI를 component로 구분하고 여러 visual state들을 정의하고 component들을 조합해서 데이터가 흐르도록 하는 걸 의미한다.

## <mark style="background: #FF5582A6;">작업할 때의 과정</mark>

mockup이나 JSON API를 갖고 있다고 가정한다.

JSON API
![[Pasted image 20240420185743.png]]
mockup
![[Pasted image 20240420185806.png]]
## <mark style="background: #FFB86CA6;">1. Break the UI into a component hierarchy</mark>

mockup에 모든 component, subcomponent마다 상자를 그리고 이름을 짓는 걸로 시작한다.

다음의 기준으로 component들을 나눌 수 있다.

1. Programming의 관점, 같은 기준으로 모든 함수와 객체들을 나눈다. (ex) single responsibility principle.
2. CSS의 관점, class selectors를 어디에 달지를 생각해라.
3. Design의 관점, design의 layer를 어떻게 구성할건지

![[Pasted image 20240420190437.png]]

1. `FilterableProductTable` (grey) 전체 app을 저장
2. `SearchBar` (blue) user input을 받음.
3. `ProductTable` (lavender) user input에 따라서 리스트를 나누거나 보여줌.
4. `ProductCategoryRow` (green) 각 카테고리의 heading
5. `ProductRow` (yellow) 각 제품의 row

- `FilterableProductTable`
    - `SearchBar`
    - `ProductTable`
        - `ProductCategoryRow`
        - `ProductRow`

이런식으로 hierarchy를 구성한다.

## <mark style="background: #FFB86CA6;">2. Build a static version in React</mark>

정적 앱을 만든다. 정적 앱에는 interactive가 없다. state도 사용하지 않고 오직 재사용 가능한 JSX만 리턴하고 자식

에게 prop을 내려주는 Component들을 만들어서 data model을 구현한다.

작은 앱에는 top-down 방식

큰 앱에는 bottom-up 방식이 유리하다.

## <mark style="background: #FFB86CA6;">3. Find the minimal but complete representation of UI state</mark>

UI를 interactive하게 하기 위해선 data model을 수정할 수 있게 해야 하는데 여기서 필요한게 state이다.

state: 앱에서 기억해야 하는 최소한의 데이터 변경 모음 (반복해서는 안된다. DRY(Don't Repeat Yourself) )

state 거르는 법

1. 변하지 않은 채로 남아 있진 않는가?
2. props를 통해서 부모로부터 전달받진 않는가?
3. 기존의 state나 props를 계산해서 만들어내진 않는가?

## <mark style="background: #FFF3A3A6;">Props vs State</mark>

Props :  함수에 넘겨주는 arg와 같다. 부모 컴포넌트가 자식 컴포넌트로 정보를 전달하게 해주고 모습을 바꾸게

해준다.

State : Component's memory와 같다. 계속 정보를 추적하고 바뀐 것의 응답으로 상호작용한다.

## <mark style="background: #FFB86CA6;">4. Identify where your state should live</mark>

React는 one-way data flow이기 때문에 어떤 컴포넌트가 어떤 state를 가질지 깔끔하지 않다. 

어디에 어떤 state를 놓을지 아는 법

1.  그 state를 기반으로 무언가 만드는 모든 component 찾기
2. 그들을 모두 자식으로 갖고 있는 가장 가까운 부모 component찾기
3. state를 어디에 놓을지 결정하기
	1. 공통 부모에 놓기
	2. 공통 부모 위에 놓기
	3. 공통 부모 위에 새 컴포넌트를 만들고 state 저장하기

## <mark style="background: #FFB86CA6;">5. Add inverse data flow</mark>

React는 form 구현에서 data flow를 명시적으로 만든다. 이건 two-way data binding보다 할게 조금 많다.

```jsx
<input value={stateName} />
// state와 input에 입력되는 값이 일치하게 된다.
// 하지만 이것만으로는 value를 입력할 수 없다.
```

```jsx
// setter함수를 하위 컴포넌트로 옮긴다.
function FilterableProductTable({ products }) {  
const [stateName, setStateName] = useState('');    
  
return (  
<div>  
<SubInput 
	stateName={stateName}
	onTextChange = {setStateName}
	/>
```

```jsx
function SubInput({stateName, onTextChange}){
	return (<input value={stateName} onChange={(e)=> onTextChange(e.target.value)}/>)
}
```

