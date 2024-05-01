React는 UI를 rendering하는 library이다.

## <mark style="background: #FF5582A6;">1. Component</mark>

UI를 만드는 기초

HTML에는 좋은 내장 tag들이 존재한다.

```html
<article>  
	<h1>My First Component</h1>  
	<ol>  
		<li>Components: UI Building Blocks</li>  
		<li>Defining a Component</li>  
		<li>Using a Component</li>  
	</ol>  
</article>
```

위 마크업은 각각 article(`<article>`), heading(`<h1>`), ordered list(`<ol>`)을 의미한다.

이 마크업은 CSS, JS와 결합하곤 하는데

React는 각 CSS, JS, HTML을 하나의 재사용 가능한 UI 요소인 `Component`로 묶는다. (위의 코드를 `Component`로 바꾸면 `<TableOfContents />`로 묶을 수 있다.)

또한 Component끼리 tag처럼 페이지를 완성할 수 있는데 React 공식 문서 페이지를 예로 들 수 있다.
```jsx
<PageLayout>
	<NavigationHeader>  
		<SearchBar />  
		<Link to="/docs">Docs</Link>  
	</NavigationHeader>  
	<Sidebar />  
	<PageContent>  
		<TableOfContents /> 
		<DocumentationText />  
	</PageContent>  
</PageLayout>
```

## <mark style="background: #FFB86CA6;">Defining a Component</mark>

기존에 JS가 있으면 좋고 없어도 말고 하던 시절에는 Markup을 만들고 JS를 거기에 뿌렸지만 React는 다른 방식으로 사용된다.

> `React는 Markup을 뿌릴 수 있는 JS 함수이다.`

```jsx
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

만드는 과정은 다음과 같다.

1. Export the Component

`export default` 는 파일의 메인 함수를 마크하고 나중에 다른 파일에서 import할 수 있게 해준다.

2. Define the function

`function Profile() {}`로 JS 함수에 이름을 짓는다. (expression 방식으로 해도 됨.)

React Component는 대문자로 시작해야 작동하는 JS 함수이다.

3. Add markup

JSX라는 문법을 이용해서 HTML처럼 쓰여진 tag를 Component에서 리턴한다.

return에 담을 statement가 하나라면 그냥 적어도 되고 아니라면 `( )` 안에 담아야 한다.

```jsx
// 하나일 때
return <img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />;

// 여러 개일 때
return (  
	<div>
		<img src="https://i.imgur.com/MK3eW3As.jpg" alt="Katherine Johnson" />  
	</div>  
);
```

## <mark style="background: #FFB86CA6;">Using a Component</mark>

위의 기초 지식들을 기반으로 Component를 짜보자.

```jsx
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

Profile이 여러개가 쓰인 Gallery라는 Component를 export한 모습이다.

## <mark style="background: #FFF3A3A6;">What the browser sees</mark>

소문자, 대문자에 주의하라.

1. `<section>`은 소문자이기에 React는 HTML Tag로 판단한다.
2. `<Profile />`은 대문자로 시작하기 때문에 React는 Component로 판단한다.

## <mark style="background: #FFB86CA6;">Nesting and organizing components</mark>

## <mark style="background: #FFF3A3A6;">주의사항</mark>

1. export하는 Component안에 Component를 정의하면 느리고 버그도 생기게 된다.

2. 모든 선언은 top level에서 해라. (자식 Component가 부모 Component로부터 데이터를 받아야 하면 prop을 사용해라.)

## <mark style="background: #FFB86CA6;">Component 심화</mark>

1. React application은 root Component에서 시작한다.

2. 이 Component는 project를 시작할 때 자동으로 생긴다.
(Next.js나 CodeSandbox를 이용하면 root Component는 `pages/index.js` 에 존재하며 이 경우 root Component를 exporting한다.)

3. 대부분의 React 앱은 처음부터 끝까지 Component를 사용하며 이는 크기와 상관 없다.

4. React based framework들은 더 나아가서 React Component들에서 HTML을 자동으로 생성한다. 이렇게 되면 JS 로드 전에 content를 볼 수 있게 된다.


## <mark style="background: #FF5582A6;">2.Importing and Exporting Components</mark>

## <mark style="background: #FFB86CA6;">The root Component file</mark>

file-based routing를 사용하는 Next.js같은 것은 모든 페이지마다 root component가 다르다.

## <mark style="background: #FFB86CA6;">Exporting and importing a component</mark>

```jsx
import Gallery from './Gallery';
```

React에서는 Gallery.js를 Gallery로 적어도 자동으로 바꿔준다.

## <mark style="background: #FFB86CA6;">Default vs named exports</mark>

1. default exportss는 하나만 가질 수 있고 named는 여러개 가질 수 있다. ![[Pasted image 20240420205621.png]]
2. export한 방법에 따라서 import하는 방법이 달라진다.

| Syntax  | Export statement                      | Import statement                        |
| ------- | ------------------------------------- | --------------------------------------- |
| Default | `export default function Button() {}` | `import Button from './Button.js';`     |
| Named   | `export function Button() {}`         | `import { Button } from './Button.js';` |
default export의 경우 import할 때 하고 싶은 이름으로 가져올 수 있다. 

보통 default export의 경우 하나의 component만 export할 때 사용하고 여러 개일 때는 named export를 사용한다.

`export default ()=>{}`의 경우 디버깅이 힘들어서 안 쓰는 것이 좋다.

## <mark style="background: #FF5582A6;">3.Writing Markup with JSX</mark>

JSX는 HTML-like markup을 js에서 쓸 수 있는 확장 문법이다.

## <mark style="background: #FFB86CA6;">JSX: Putting markup into JavaScript</mark>

이전에는 각각의 파일에 content를 위한 HTML, style을 위한 CSS, logic을 위한 JS 가 있었다.

![[Pasted image 20240420221258.png]]
하지만 웹이 interactive됨에 따라서 logic이 점점 content를 결정했다. JS가 HTML의 영역으로 들어오기 시작했다.

![[Pasted image 20240420221406.png]]
rendering logic과 markup을 함께 두면 특정 변화마다 동기화를 확실히 할 수 있고 details들 (서로 다른 컴포넌트의 세부 정보들) 은 따로 분리시켜서 관리할 수 있다.

## <mark style="background: #FFB86CA6;">The Rules of JSX</mark>

## <mark style="background: #FFF3A3A6;">1. Return a single root element</mark>

 `<></>` (Fragment) 나 `<div></div>` 를 이용해서 하나의 element로 return을 해야한다.

JSX는 내부적으로 JS object이다. 한 함수에서 두개의 Object를 리턴할 수 없기 때문에 div나 Fragment로 묶는 것이다.

## <mark style="background: #FFF3A3A6;">2. Close all the tags</mark>

`<img>`처럼 self-closing tags의 경우 `<img/>`으로 바꿔야 한다.

`<li></li>` 역시 똑같이 닫아야 한다.

## <mark style="background: #FFF3A3A6;">3. camelCase most of the things!</mark>

JSX는 JS로 바뀌고 JSX에 쓰여진 attribute는 JS 객체의 key로 바뀐다.

component에서 해당 key를 variable로 읽을 수 있는데 JS 특성상 제한이 있다.

dash를 못 쓴다거나 `class` 같은 예약어를 못 쓰는 경우가 있다.

그래서 React에서는 HTML SVG 속성들이 다 camelCase로 바꿔져 있다.

(`aria-*`, `data-*` 의 경우만 예외로 HTML에 dash를 가지고 있다.)

## <mark style="background: #FFB86CA6;">JSX Converter</mark>

https://transform.tools/html-to-jsx

이걸 이용해서 HTML을 JSX로 바꿀 수 있다.

## <mark style="background: #FF5582A6;">4. JavaScript in JSX with Curly Braces</mark>

curly braces`{}` 를 이용해서 JSX에 JS를 사용할 수 있다.

## Passing strings with quotes

`''` 나 `""`을 이용해서 strings을 사용한다.

```jsx
export default function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/7vQD0fPs.jpg"
      alt="Gregorio Y. Zara"
    />
  );
}
```

만약 dynamically하게 사용하고 싶다면 `{}`을 사용하면 된다.

```jsx
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

## Using curly braces: A window into the JS World

JSX는 JS의 확장 문법이기 때문에 `{}`을 사용하여 JS 사용이 가능하다.

```jsx
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

curly braces를 사용하는 곳

1. text로써 JSX tag안에 사용

```jsx
<h1>{name}'s To Do List</h1>
```

2. attribute로써 =뒤에 사용

```jsx
src={avatar} // avatar를 variable로써 읽는다.
// src="{avatar}"는 {avatar}를 읽는다.
```

## Using "double curlies": CSS and other objects in JSX

CSS object나 JS의 object의 경우 똑같이 `{}`안에 넣기 떄문에 `{{}}`로 JSX안에 넣어야 한다.

```jsx
export default function TodoList() {
  return (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Improve the videophone</li>
      <li>Prepare aeronautics lectures</li>
      <li>Work on the alcohol-fuelled engine</li>
    </ul>
  );
}
```
![[Pasted image 20240427143522.png]]

inline style properties는 camelCase로 작성되어야 한다.

## More fun with JS objects and curly braces

객체 안에 프로퍼티가 객체일 경우에도 그냥 접근해서 사용할 수 있다.

JSX는 data와 logic을 JS로 구조화시키는 최적의 templating language이다.

## 5. Passing Props to a Component

React Component는 props를 이용해서 서로 communicate한다.

모든 부모 component는 자식 component로 prop을 이용해서 정보를 전달한다.

## Familiar props

props를 이용해서 JSX tag에 친숙한 정보를 전달한다.

```jsx
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
      width={100}
      height={100}
    />
  );
}

export default function Profile() {
  return (
    <Avatar />
  );
}
```

## Passing props to a component

1. 자식 component로 props pass

```jsx
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```

2. 자식 component에서 props를 읽는다.

```jsx
function Avatar({ person, size }) {
  // person and size are available here
}
```

이로써 작동하게 된다.

Component는 props라는 객체를 받는다.

그래서 `{}` destructuring을 이용해서 props안에 있는 property들을 따로 받을 수 있는 것이다.

## Specifying a default value for a prop

값이 없을 때 fall back on 하기 위해서 default value를 설정하면 좋다.

` = ` 을 이용하면 가능하다.

```jsx
function Avatar({ person, size = 100 }) {  
	// ...  
}
```

이제 default value는 100이 된다.

undefined를 size에 넣었거나 size를 비웠으면 해당 default value가 할당되게 된다.

## Forwarding props with the JSX spread syntax

```jsx
function Profile({ person, size, isSepia, thickBorder }) {
  return (
    <div className="card">
      <Avatar
        person={person}
        size={size}
        isSepia={isSepia}
        thickBorder={thickBorder}
      />
    </div>
  );
}
```

prop 객체를 전혀 사용하지 않을 것이고 그대로 자식 component로 내려준다고 하면

```jsx
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

spread syntax로 정리할 수 있다.

하지만 이는 비효율적 코드 사용이며 component를 나눠야 한다는 뜻이기도 하다.

## Passing JSX as children

자식 component를 부모 component 안에 nest한다면 해당 자식 component는 부모 component의 children prop에 할당되게 된다.

```jsx
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}
```
![[Pasted image 20240427214840.png]]

## How props change over time

1. props는 static하지 않다. 

2. props는 immutable하다.
unchangable하다. 

그래서 user interaction이나 새로운 정보에 대한 response로 props를 바꿔야 하면 부모 component에 다른 props를 달라고 요청해야 한다. 

그러면 JS는 old props를 치우고 해당 메모리 공간을 되찾게 된다.

## 6. Conditional Rendering

condition에 따라서 다른 걸 보여주려면 `&&`나 `? :` operator를 사용한다.

## Conditionally returning JSX

`if`을 이용해서 조건에 따라 다른 JSX를 return할 수 있다.

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>; // 조건에 따라 리턴
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

## Conditionally returning nothing with `null`

`{}`안에 null을 담으면 아무것도 출력되지 않는다.

```jsx
function Item({ name, isPacked }) {
  if (isPacked) {
    return null; // null을 리턴하면 아무것도 출력되지 않는다.
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

## Conditional (ternary) operator (`? :`)

js 문법인 ternary를 이용해서 간단하게 표현할 수 있다.

```jsx
return (
  <li className="item">
    {isPacked ? name + ' ✔' : name}
  </li>
);
```

`if`를 이용한 방식과 `? :`을 이용한 방식은 아예 같다.

if가 `li`의 instance를 두개 사용한다고 생각할 수 있지만 사실 JSX 요소는 `instance`도 아니고 내부의 state를 갖고 있지도 진짜 DOM node도 아니기 때문에 두 방식이 아예 같다고 보면 된다.

하지만 너무 복잡해질 수 있기 때문에 nested conditional markup이 많으면 변수, 함수 등으로 분리시켜야 한다.

## Logical AND operator (`&&`)

조건이 true일 때 JSX를 render하거나 false일 때 아무것도 안 render하려 할때 사용한다.

```jsx
return (
  <li className="item">
    {name} {isPacked && '✔'}
    {/* “if `isPacked`, then (`&&`) render the checkmark, otherwise, render nothing”*/}
  </li>
);
```

React에서는 `false`,`null`,`undefined`는 그냥 JSX tree의 구멍이기 때문에 `&&` 왼쪽 expression이 `false`여도 아무것도 render하지 않는다.

단, `0`의 경우 rendering이 정상적으로 되기 때문에 주의해야 한다.

## Conditionally assigning JSX to a variable

조건 연산자가 일반적인 코드 작성을 방해하면 (get in the way of writing plain code) `if`와 `let`을 이용한 변수를 통해서 유동적인 코드를 작성할 수 있다.

```jsx
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = ( 
// let이 포함된 변수는 재할당이 가능하기 때문에 arbitrary JSX도 할당시킬 수 있다.
      <del>
        {name + " ✔"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

## 7. Rendering Lists

JS Array method를 이용해서 array of data 를 array of components로 만들 수 있다.

## Rendering data from arrays

다른 content인데 같은 component를 사용할 경우 `map`을 이용해서 간단하게 처리할 수 있다.

```jsx
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

## Filtering arrays of items

`filter`와 `map`을 이용해서 다양한 방식으로 array를 응용할 수 있다.

```jsx
export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  const listItems = chemists.map(person =>
    <li>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

## pitfall

arrow function은 그냥 ` => `만 있으면 return을 자동으로 시켜준다.

단 ` => { ` 이런식으로 쓰일 경우에는 block body를 가진 것으로 취급되어 return을 명시적으로 써줘야 한다.

## Keeping list items in order with `key`

```terminal
Warning: Each child in a list should have a unique “key” prop.
```

list item에 key을 안 넣으면 발생하는 에러이다. ( `map()`안에 있는 것도 해당된다. )

key에는 unique한 string이나 number가 들어가야 한다.

`key`는 React한테 각 component와 상응하는 array item이 무엇인지 알려주고 그걸로 나중에 match시킨다.
(이동, 삽입, 제거에 쓰이게 된다.)

key를 생성시키는 것보다 데이터 자체에 담겨 있어야 한다.

## Displaying several DOM nodes for each list item

`Fragment`가 필요한 상황 (각 아이템이 여러 DOM을 리턴해야 할때)에서 `key`를 지정하기 위해서는 `<></>`가 아닌 `<Fragment></Fragment>`를 사용하고 안에 key을 넣어줘야 한다.

```jsx
import { Fragment } from 'react';

// ...

const listItems = people.map(person =>
  <Fragment key={person.id}>
    <h1>{person.name}</h1>
    <p>{person.bio}</p>
  </Fragment>
);
```

## Where to get your `key`

## Data from a database

database key/ ID를 사용하면 된다.

## Locally generated data

아이템을 생성할 때 incrementing counter, `crypto.randomUUID()` 혹은 `uuid`같은 package를 사용하면 좋다.

## Rules of keys

1. siblings 끼리 고유해야 한다. 다른 배열끼리는 괜찮음.
2. key는 바뀌거나 목적을 잃어선 안된다. rendering시 생성되어선 안된다.

## Why does React need keys?

array의 JSX key와 folder의 file 이름은 비슷한 목적을 가지고 있다.
(siblings 끼리 인식하기 위해서 필요하다.)

잘 고른 key는 array의 위치 뿐만 아니라 더 다양한 정보를 제공할 수 있다.
reordering되어서 위치가 바뀌더라도 `key`는 React에게 lifetime동안 item을 인식시켜준다.

## pitfall

1. React는 `key`로 아무것도 지정 안했을 때 index를 key로 쓴다. 즉 index를 key로 넣는건 아무 효과가 없다.

2. Do not generate keys on the fly

`key={Math.random()}` 이렇게 쓰면 key가 렌더링마다 바뀌게 된다.
component와 DOM이 계속 재생성되게 된다. 그래서 stable ID를 넣어야 한다.

3. key는 prop으로 인정되지 않는다.

React 스스로만 쓰는 거지 prop으로 쓰이는 것이 아니다. 만약 Component에 key에 넣은 value가 필요하다면 다른 prop을 만들어서 그 value를 넣어서 써야한다.

```jsx
<Profile key={id} userId={id} /> // key는 못 받음.
```

## Component의 key

Component에 key가 필요할 때는 Component 자체에 key를 넣어야 한다. root div에 넣고 이런게 아니라 component 자체에 key를 넣어주면 된다.

## 4번 문제

JSX array를 variable에 담으면 shift같은 메소드로 쉽게 사용할 수 있다.

```jsx
const poem = {
  lines: [
    'I write, erase, rewrite',
    'Erase again, and then',
    'A poppy blooms.'
  ]
};

export default function Poem() {
  let output = [];

  // Fill the output array
  poem.lines.forEach((line, i) => {
    output.push(
      <hr key={i + '-separator'} />
    );
    output.push(
      <p key={i + '-text'}> {/*suffix를 이용해서 앞의 push에 있는 key랑 다르게 설정*/}
        {line}
      </p>
    );
  });
  // Remove the first <hr />
  output.shift();

  return (
    <article>
      {output}
    </article>
  );
}
```

## 8. Keeping Components Pure

일부 JS 함수는 Pure하다. Pure한 JS 함수는 계산만 수행한다.

Component 역시 Pure하게 작성한다면 코드가 커져도 안정성을 유지한다.

이를 위해선 몇가지 규칙이 존재한다.

## Purity: Components as formulas

## pure function

1. 그것 고유의 비즈니스만을 다룬다. 호출 전에 존재했던 객체나 변수를 바꾸지 않는다.
2. same inputs, same outputs. 같은 input이면 항상 같은 output이 나온다.

pure function의 예로는 수학의 formula가 있다.

```Math
y=2x

x=2 then y=4
x=3 then y=6 // 같은 input이면 항상 같은 output
```

JS로 표현하면
```js
function double(number){
	return 2*number;
}
```

React는 모든 component가 pure function인 것으로 가정한다.

Recipe로 생각하면 된다. ingredient를 introduce하지 않으면 항상 같은 dish가 나오게 된다.

![[Pasted image 20240429120420.png]]

## Side Effects: (un)intended consequences

호출 전에 지정한 함수나 변수를 바꾸게 되면 impure한 function이다.

```jsx
let guest = 0;

function Cup() {
  // Bad: changing a preexisting variable!
  guest = guest + 1;
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

