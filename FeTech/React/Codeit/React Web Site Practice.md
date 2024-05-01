## <mark style="background: #FF5582A6;">React Router</mark>

html파일을 여러개 만들어서 이동시키는 방법이 아닌 컴포넌트를 이용하는 React의 방식

## <mark style="background: #FFB86CA6;">React Router v6</mark>

리액트 컴포넌트로 페이지를 나누는 라이브러리

## <mark style="background: #FFF3A3A6;">설치</mark>

`npm install react-router-dom localforage match-sorter sort-by`

vite에 맞게 설치

## <mark style="background: #FFB86CA6;">Router</mark>

리액트 라우터에서 사용하는 것들을 갖고 있다.

내부적으로 Context Provider이다. ( 해당 기능을 쓰기 위해서는 Router 컴포넌트 안에서 사용해야 한다. )

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import {
// 라우터의 한 종류
  createBrowserRouter,
  createRoutesFromElements,
  Route,
  RouterProvider,
} from "react-router-dom";
  
import App from "./app/App";
  
import "./index.css";
import ErrorPage from "./pages/ErrorPage/ui/ErrorPage";
import { Items } from "./pages/items";
  
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<App />}>
      <Route index element={<Items />} />
      <Route path="*" element={<ErrorPage />} />
    </Route>
  )
);
  
ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

## <mark style="background: #FFB86CA6;">Routes, Route</mark>

```jsx
<Route path="/">
	<Route index element={<HomePage />} />
	<Route path="courses" element={<CourseListPage />} />
	<Route path="courses/1" element={<CoursePage />} />
	<Route path="*" element={<NotFoundPage />} />
	// route에 접근할 때 해당 path가 존재하다면 해당 element를 뽑아낸다. 
</Route>
```

switch 문과 같다고 생각하면 편하다. 

각 Route에는 path Prop에는 경로가 들어가고 ,element Prop에는 해당하는 JSX가 들어간다.

리액트에서만 존재하는 Component이기 때문에 div같은걸 랜더링하진 않는다.

## <mark style="background: #FFB86CA6;">Link</mark>

React Router에서 a 태그 대신에 사용하는 것이다.

```jsx
import {Link} from 'react-router-dom'
//...
<Link to="/">홈페이지</Link>
<Link to="/courses">수업 탐색</Link>
<Link to="/questions">커뮤니티</Link>
```

`to`라는 prop으로 Route를 지정한다.

slug라는 것으로 Route를 표현하기도 한다. (그냥 영어 단어임. slug)

a와 다르게 network request를 보내지 않음.

## <mark style="background: #FFB86CA6;">NavLink</mark>

Link와 같지만 style이라는 prop에 [[React Web Development Foundamental#^c63c61|인라인 스타일]]로 함수를 담을 수 있다.

```jsx
function callback({isActive}){
	return{
		textDecoration: isActive ? 'underline' : undefined,
	}
}

<NavLink to="route" style={callback}>
```

## <mark style="background: #FFB86CA6;">Nested Route</mark>

```jsx
const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    errorElement: <ErrorPage />,
    children: [
      {
        path: "contacts/:contactId",
        element: <Contact />,
      },
    ],
  },
]);
```

Route하위에 Route를 만들면  Route를 이어서 받게 된다.

index를 붙히면 기본 라우터로 설정됨. (`/courses`)

## <mark style="background: #FFF3A3A6;">Outlet</mark>

```jsx
// main.jsx
import { Outlet } from "react-router-dom";

export default function Root() {
  return (
    <>
      {/* all the other elements */}
      <div id="detail">
        <Outlet />
      </div>
    </>
  );
}
```

Outlet으로 children들을 아래로 내린다.

## <mark style="background: #FFB86CA6;">Dynamic path</mark>

Route path에 Parameter를 넣어서 일일이 넣지 않아도 되게 바꿈.

```jsx
 <Route path="courses">
	<Route index element={<CourseListPage />} />
	// Dynamic path로 변경
	<Route path=":courseSlug" element={<CoursePage />} /> 
	// courseSlug라는 변수로 페이지를 받아옴. (파라미터라고 부름)
</Route>
```

이제 courses의 하위 URL로 아무거나 넣어도 그 아무거나는 courseSlug로 저장되게 된다.

## <mark style="background: #FFF3A3A6;">useParams</mark>

현재 경로의 Parameter들이 들어있다.

```jsx
import { useParams } from 'react-router-dom';

function CoursePage() {
 // Params에 있는 courseSlug을 꺼내서 courseSlug로 넣음.
  const { courseSlug } = useParams();
  const course = getCourseBySlug(courseSlug);
```

아까 저장된 courseSlug를 꺼내서 slug로 데이터를 찾고 course에 담아준다.

## <mark style="background: #FFB86CA6;">없는 페이지 처리</mark>

Route에서 매치되는 것이 하나도 없는데 접근할 경우 아무것도 표시하지 않게 된다.

```jsx
<Route path="*" element={<NotFountPage>} />
```

## <mark style="background: #FFB86CA6;">에러 페이지 처리</mark>

```jsx
import { useRouteError } from "react-router-dom";

export default function ErrorPage() {
// react-router에서 제공하는 에러
  const error = useRouteError();
  console.error(error);

  return (
    <div id="error-page">
      <h1>Oops!</h1>
      <p>Sorry, an unexpected error has occurred.</p>
      <p>
        <i>{error.statusText || error.message}</i>
      </p>
    </div>
  );
}
```

## <mark style="background: #FFB86CA6;">useSearchParams</mark>

쿼리스트링을 다루는데 사용된다.

기본적으로 name=value 방식으로 쿼리스트링을 보내는데, onSubmit 안의 함수에 의해서 막히게 되고

setSearchParams를 이용해서 주소창에 쿼리스트링을 바꾼다.

```jsx
import {useSearchParams} from 'react-router-dom';

// useState과 유사하지만 searchParams에는 객체가 들어가 있다.
const [searchParams, setSearchParams]=useSearchParams();

// 이런 식으로 객체에서 꺼낼 수 있음.
const initKeyword = searchParams.get('keyword') // 쿼리스트링이 없을 경우 null이 나오게 된다.
const [keyword, setKeyword] = useState(initKeyword || '');
const courses = getCourses(initKeyword);

//...

<form onSubmit={(e)=>{e.preventDefault(); setSearchParams(keyword ? { keyword, } : {} );}}>
	<input
		name="keyword"
		value={keyword}
	></input>
</form>

//...

{initKeyword && <>{검색어가 없을 때 일어나는 일}</>}
```

이렇게 searchParams로 보낸 건 뽑아와서 다른 곳에서 사용할 수 있다.

## <mark style="background: #FFB86CA6;">redirect</mark>

사용자가 잘못된 경로로 갈 경우 개발자가 입력한 경로로 이동시킬 수 있다.

```jsx
import {Navigate} from 'react-router-dom';
//...
if (went wrong path){
	return <Navigate to="RoutePath"/> 
	// RoutePath에 임의로 경로 지정
}
```

## <mark style="background: #FFB86CA6;">useNavigate</mark>

특정한 코드의 실행이 끝나고  나서 페이지를 이동시키고 싶을 때 사용

```jsx
import {Navigate} from 'react-router-dom'

//...
const navigate = useNavigate();
// ...
const function = ()=>{
	navigate('/Route Path') // /Route Path로 이동한다.
}
```

## <mark style="background: #FFB86CA6;">react-helmet</mark>

리액트스럽게 title을 변경하는 방법

```jsx
import {Helmet} from "react-helmet";

// ...

 <Helmet> // head을 덮어 쓴다.
	<meta charSet="utf-8" />
	<title>My Title</title>
	<link rel="canonical" href="http://mysite.com/example" />
</Helmet>
```

## <mark style="background: #FF5582A6;">Rendering</mark>

## <mark style="background: #FFB86CA6;">Client-side Rendering</mark>

웹브라우저가 JS로 html을 만드는 것

페이지 이동시 똑같은 html을 랜더링하고 클라이언트 단에서 JS파일만 스위칭하여 페이지를 이동한 것처럼 보이게 하는 기법

## <mark style="background: #FFB86CA6;">Server-side Rendering</mark>

서버에서 HTML을 만들고 리스폰스로 보내주는 것

## <mark style="background: #FFB86CA6;">Static Site Generation</mark>

미리 HTML을 만들어서 서버를 배포하는 것

Static : HTML을 파일로 만든다. (새로 배포하지 않는 한, 서버에서 보내주는 HTML이 달라지지 않는다.)

정적인 사이트에 동적인 데이터를 가져옴

## <mark style="background: #FFB86CA6;">Rendering 관련 React 기술</mark>

## <mark style="background: #FFF3A3A6;">Next.js</mark>

서버사이드 렌더링을 대신 구현해주는 기술

## <mark style="background: #FFF3A3A6;">Gatsby</mark>

정적 사이트 생성, 리액트 코드를 미리 렌더링해서 프로젝트를 HTML파일로 만든다.

## <mark style="background: #FFF3A3A6;">React Native</mark>

JS로 된 리액트 코드를 서버나 클라이언트에서 모바일 앱으로 만들어준다.