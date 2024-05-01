컴포넌트를 만들면서 해당 컴포넌트의 스타일을 작성하는 기술

컴포넌트를 만드는 것 처럼 CSS를 만든다.

## <mark style="background: #FF5582A6;">기존 방식의 문제점</mark>

1. 어떤 컴포넌트에 Style을 입히기 위해 클래스 하나를 지정해서 CSS를 할당했다고 보자.

여기서 다른 컴포넌트에서 해당 클래스 이름을 쓰면 의도치 않게 적용 되버린다.
이를 위해 클래스가 안 겹치게 해야 되는데 프로젝트가 클수록 불가능에 가깝다.

2. 재사용이 어렵다.
## <mark style="background: #FFB86CA6;">Styled Component의 해결방법</mark>

클래스 이름을 아예 쓰지 않는 방법을 택했다.

```jsx
const StyledApp = styled.div`
  background-color: #000000;
`;

const Dashboard = styled.div`
  font-size: 16px;
`;

function App() {
  return (
    <StyledApp>
      <Dashboard> ... </Dashboard>
    </StyledApp>
  );
}
```

재사용을 위해서 변수를 사용하는 방식을 사용했다.

```jsx
// shadows.js
import { css } from 'styled-components';

const shadow20 = css`
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.2);
`;
const shadow40 = css`
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.4);
`;

// Card.js
import { shadow20 } from '../shadows';

const Card = styled.div`
  ${shadow20}
  ...(다른 CSS 코드)
`;
export default Card;
```

## <mark style="background: #FF5582A6;">설치</mark>

```terminal
npm install styled-components
```

## <mark style="background: #FF5582A6;">문법</mark>

## <mark style="background: #FFB86CA6;">코드 분석</mark>

```jsx
// styled-components에서 default import로 styled를 가져왔다.
import styled from 'styled-components'

// styled.tagNamee 방식으로 태그를 가져온다. 
// 지금은 button Tag 사용
const Button = styled.button`
	background-color: #6750a4;
	border: none;
	color: #ffffff;
	padding: 16px;
`;

export default Button;

//다른 파일
import Button from '~'
//...
<Button> Hello Styled Component </Button>
```

## <mark style="background: #FFB86CA6;">Nesting</mark>

## <mark style="background: #FFF3A3A6;">& 선택자</mark>

```jsx
import styled from 'styled-components';

const Button = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;

  &:hover,
  &:active {
    background-color: #463770;
  }
`;

export default Button;
```

&:hover는 Button:hover와 같은 뜻이다.

## <mark style="background: #FFB86CA6;">컴포넌트 선택자</mark>

```jsx
const Icon = styled.img` width: 16px; height: 16px; `;

const Button = styled.button`
  background-color: #6750a4;
  border: none;
  color: #ffffff;
  padding: 16px;

  & ${Icon} { // 컴포넌트 추가
    margin-right: 4px;
  }

  &:hover,
  &:active {
    background-color: #463770;
  }
`;
```

& ${Icon}은 Button Icon 과 같은 뜻이다. ${Icon}으로도 쓴다.

## <mark style="background: #FFB86CA6;">다이나믹 스타일링</mark>

styled Component로 만든 Component에는 prop을 내려줄 수 있다.

styled Component에서 prop은 객체 형태로 받기 때문에 literal string에서 함수형에 destructuring된 파라미터를 넣

어서 styled Component가 함수형을 실행 후 알아서 값을 리턴하게 해야함.

```jsx
// Button.jsx
import styled from 'styled-components';

const SIZES = {
  large: 24,
  medium: 20,
  small: 16,
};

const Button = styled.button`
  background-color: #6750a4;
  border: none;
  border-radius: ${({ round }) => round ? `9999px` : `3px`}; 
  color: #ffffff;
  font-size: ${({ size }) => SIZES[size] ?? SIZES['medium']}px;
  padding: 16px;

  &:hover,
  &:active {
    background-color: #463770;
  }
`;

export default Button;

// App.jsx
import styled from 'styled-components';
import Button from './Button';

const Container = styled.div`
  ${Button} { // 하위 styled Component를 직접 넣으면 자식 취급 받는다.
    margin: 10px;
  }
`;

function App() {
  return (
    <Container>
      <h1>기본 버튼</h1>
      <Button size="small">small</Button> // size prop에 small을 넣음.
      <Button size="medium">medium</Button>
      <Button size="large">large</Button>
      <h1>둥근 버튼</h1>
      <Button size="small" round> // round를 넣어서 true로 만듬.
        round small
      </Button>
      <Button size="medium" round>
        round medium
      </Button>
      <Button size="large" round>
        round large
      </Button>
    </Container>
  );
}

export default App;
```

styled-component의 리터럴 문자열 안에 ${} 으로 prop을 넣어주면 이 컴포넌트를 사용하는 컴포넌트에서 prop으로 내려주기만 하면 연결되어 작동한다.

## <mark style="background: #FFF3A3A6;">일반적으로 값을 넣을 때는 리터럴 문자열과 같은 문법이기에 가능한 작동방식</mark>

```jsx
const SIZES = {
  large: 24,
  medium: 20,
  small: 16
};

const Button = styled.button`
  ...
  font-size: ${SIZES['medium']}px;
`;
```

## <mark style="background: #FFF3A3A6;">함수를 넣을때는 내부적으로 Styled-component가 처리해준다.</mark>

```jsx
const SIZES = {
  large: 24,
  medium: 20,
  small: 16
};

const Button = styled.button`
  ...
  font-size: ${(props) => SIZES[props.size]}px;
`;

// destructuring
font-size: ${({ size }) => SIZES[size] ?? SIZES['medium']}px;
// undefined가 들어가지 않게 하기 위해 기본값 생성 (Nullish coalescing operator)
```

## <mark style="background: #FFB86CA6;">다양한 연산자들</mark>

```jsx
// nullish coalescing operator
font-size: ${({ size }) => SIZES[size] ?? SIZES['medium']}px;

// &&
const Button = styled.button`
  ...
  ${({ round }) => round && `
      border-radius: 9999px;
    `}
`;

// ternary operator
border-radius: ${({ round }) => round ? `9999px` : `3px`};
```

## <mark style="background: #FFB86CA6;">스타일 상속</mark>

이미 만들어진 컴포넌트에 스타일을 입히는 방법

## <mark style="background: #FFB86CA6;">styled() 함수</mark>

```jsx
// App.jsx
import styled from 'styled-components';
import Button from './Button';

// 버튼 스타일을 갖고와서 상속
const SubmitButton = styled(Button)`
  background-color: #de117d;
  display: block;
  margin: 0 auto;
  width: 200px;

  &:hover {
    background-color: #f5070f;
  }
`;// 업데이트 한다.

function App() {
  return (
    <div>
      <SubmitButton className="hi">계속하기</SubmitButton>
    </div>
  );
}

export default App;
```

## <mark style="background: #FFF3A3A6;">ex) JSX 컴포넌트에 styled 사용해서 상속</mark>

```jsx
// TermsOfService.jsx
function TermsOfService({ className }) { // className이 prop에 있어야한다.
  return (
    <div className={className}> //className 적용
      <h1>㈜코드잇 서비스 이용약관</h1>
      <p>
        환영합니다.
        <br />
        Codeit이 제공하는 서비스를 이용해주셔서 감사합니다. 서비스를
        이용하시거나 회원으로 가입하실 경우 본 약관에 동의하시게 되므로, 잠시
        시간을 내셔서 주의 깊게 살펴봐 주시기 바랍니다.
      </p>
      <h2>제 1 조 (목적)</h2>
      <p>
        본 약관은 ㈜코드잇이 운영하는 기밀문서 관리 프로그램인 Codeit에서
        제공하는 서비스를 이용함에 있어 이용자의 권리, 의무 및 책임사항을
        규정함을 목적으로 합니다.
      </p>
    </div>
  );
}

export default TermsOfService;

// App.jsx
import styled from 'styled-components';
import Button from './Button';
import TermsOfService from './TermsOfService'; // jsx로 만든 컴포넌트를 가져온다.

// styled를 이용해서 컴포넌트에 스타일을 넣은 컴포넌트를 만든다.
const StyledTermsOfService = styled(TermsOfService)`
  background-color: #ededed;
  border-radius: 8px;
  padding: 16px;
  margin: 40px auto;
  width: 400px;
`;

// Button이라는 스타일 컴포넌트도 상속 가능
const SubmitButton = styled(Button)`
  background-color: #de117d;
  display: block;
  margin: 0 auto;
  width: 200px;

  &:hover {
    background-color: #f5070f;
  }
`;

function App() {
  return (
    <div>
      <StyledTermsOfService />
      <SubmitButton className="클래스">계속하기</SubmitButton>
    </div>
  );
}

export default App;
```

내부적으로 styled component는 className을 따로 생성하는데 JSX로 만든 Component에는 className이 없기 때문에 따로 prop에 className을 넣어줘야 한다. (이제 바뀜 )

```jsx
function TermsOfService({ className }) {
  return (
    <div className={className}>
      ...
    </div>
  );
}

const StyledTermsOfService = styled(TermsOfService)`
	background-color: #ededed; 
	border-radius: 8px; 
	padding: 16px; 
	margin: 40px auto; 
	width: 400px; 
`;
```

## <mark style="background: #FFB86CA6;">css 함수</mark>

```jsx
import styled, { css } from 'styled-components';

const SIZES = {
  large: 24,
  medium: 20,
  small: 16
};

const fontSize = css`
  font-size: ${({ size }) => SIZES[size] ?? SIZES['medium']}px;
`;

const Button = styled.button`
  ...
  ${fontSize}
`;

const Input = styled.input`
  ...
  ${fontSize}
`;
```

styled.tagName 대신에 css를 넣어서 css 뭉치를 변수로 관리할 수 있다.

이는 다른 곳에서 재사용할 때 리터럴 문자열에 그대로 넣으면 된다.

## <mark style="background: #FFB86CA6;">Global Style</mark>

`createGlobalStyle`을 이용해서 전체 코드에 적용할 스타일을 적용할 수 있다.

```jsx
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  * {
    box-sizing: border-box;
  }

  body {
    font-family: 'Noto Sans KR', sans-serif;
  }
`;

function App() {
  return (
    <>
      <GlobalStyle />
      <div>글로벌 스타일</div>
    </>
  );
}

export default App;
```

내부적으로는 해당 style을 head에 통째로 넣는 것이다.

![[Pasted image 20240420105517.png]]

## <mark style="background: #FFB86CA6;">애니메이션</mark>

## <mark style="background: #FFF3A3A6;">keyframes</mark>

움직임의 기준이 되는 프레임만 만들고 그 사이는 자동으로 채워 넣는 방식을 쓰는 지금 그 기준을 keyframe이라고 한다.

```css
@keyframes bounce { // @keyframes 로 선언 후에
  0% {
    transform: translateY(0%);
  }

  50% {
    transform: translateY(100%);
  }

  100% {
    transform: translateY(0%);
  }
}

.ball {
  animation: bounce 1s infinite; // animation으로 사용
  background-color: #ff0000;
  border-radius: 50%;
  height: 50px;
  width: 50px;
}
```

## <mark style="background: #FFF3A3A6;">keyframes 함수</mark>

기존 keyframes를 사용한 css 파일

```css
@keyframes placeholder-glow {
  50% {
    opacity: 0.2;
  }
}

.placeholder {
  animation: placeholder-glow 2s ease-in-out infinite;
}

.placeholder-item {
  background-color: #888888;
  height: 20px;
  margin: 8px 0;
}

.a {
  width: 60px;
  height: 60px;
  border-radius: 50%;
}

.b {
  width: 400px;
}

.c {
  width: 200px;
}
```

styled Component로 component화 시킨 예시

```jsx
// placeHolder.jsx
import styled, { keyframes } from 'styled-components';

const placeholderGlow = keyframes` // keyframes 함수 사용
  50% {
    opacity: 0.2;
  }
`;

export const PlaceholderItem = styled.div`
  background-color: #888888;
  height: 20px;
  margin: 8px 0;
`;

const Placeholder = styled.div`
  animation: ${placeholderGlow} 2s ease-in-out infinite;
  // 리터럴${}로 animation을 주입한다.
`;

export default Placeholder; // export



// App.jsx
import styled from 'styled-components';
import Placeholder, { PlaceholderItem } from './Placeholder';

const A = styled(PlaceholderItem)`
  width: 60px;
  height: 60px;
  border-radius: 50%;
`;

const B = styled(PlaceholderItem)`
  width: 400px;
`;

const C = styled(PlaceholderItem)`
  width: 200px;
`;

function App() {
  return (
    <div>
      <Placeholder>
        <A />
        <B />
        <C />
      </Placeholder>
    </div>
  );
}

export default App;
```

keyframe 함수를 담은 변수는 내부적으로 아래와 같다.

```Terminal
{
    id: "sc-keyframes-bEnYbJ"
    inject: ƒ (e, t)
    name: "bEnYbJ"
    rules: "\n  50% {\n    opacity: 0.2;\n  }\n"
    toString: ƒ ()
}
```

이렇기 때문에 styled나 css함수, 함수 리터럴 등으로 받아야 한다.

## <mark style="background: #FFB86CA6;">Theme</mark>

## <mark style="background: #FFF3A3A6;">ThemeProvider</mark>

context 기반으로 styled components에서 테마를 구성할 때 사용하는 컴포넌트이다.

```jsx
import { ThemeProvider } from "styled-components";
import Button from "./Button";

function App() {
  const theme = {
    primaryColor: '#1da1f2',
  };

  return (
  // 이제부터 하위 컴포넌트는 theme이라는 객체를 prop으로 사용할 수 있다.
    <ThemeProvider theme={theme}>
      <Button>확인</Button>
    </ThemeProvider>
  );
}

export default App;

// 사용 예시 (하위 컴포넌트)
const Button = styled.button`
  background-color: ${({ theme }) => theme.primaryColor};
  /* ... */
`;
```

state를 이용해서 더 정교하게 테마 값을 바꿀 수 있다.

```jsx
import { useState } from 'react';
import { ThemeProvider } from 'styled-components';
import Button from './Button';

function App() {
  const [theme, setTheme] = useState({
    primaryColor: '#1da1f2',
  });

// 테마값 설정
  const handleColorChange = (e) => {
    setTheme((prevTheme) => ({
      ...prevTheme,
      primaryColor: e.target.value,
    }));
  };

  return (
    <ThemeProvider theme={theme}>
      <select value={theme.primaryColor} onChange={handleColorChange}>
        <option value="#1da1f2">blue</option>
        <option value="#ffa800">yellow</option>
        <option value="#f5005c">red</option>
      </select>
      <br />
      <br />
      <Button>확인</Button>
    </ThemeProvider>
  );
}

export default App;
```

테마 설정 페이지를 만들려고 일반 컴포넌트에서 참조할 일이 생긴다면 ThemeContext를 'styled-components'에서 불러오고 useContext를 이용해서 Context를 가져오면 된다.

```jsx
import { useContext } from 'react';
import { ThemeContext } from 'styled-components';

// ...

function SettingPage() {
  const theme = useContext(ThemeContext); // { primaryColor: '#...' }
  // ThemeProvider의 prop으로 setter 함수를 넣으면 state를 일반 컴포넌트에서 관리할 수 있다.
}
```

하위 컴포넌트들은 함수를 사용해서 prop을 내려받을 수 있다.

```jsx
//...
const GlobalStyle = createGlobalStyle`
  body {
    background-color: ${({ theme }) => theme.backgroundColor};
    color: ${({ theme }) => theme.color};
  }
`;
//...
const StyledInput = styled.input`
  font-size: ${({ size }) => SIZES[size] ?? SIZES['medium']}px;
  border: 2px solid ${({ error }) => (error ? `#f44336` : `#eeeeee`)};
  border-radius: ${({ round }) => (round ? `9999px` : `4px`)};
  background-color: ${({ theme }) => theme.backgroundColor};
  color: ${({ theme }) => theme.color};
  outline: none;
  padding: 16px;
  position: relative;

  ${({ error }) =>
    !error &&
    `
    &:focus {
      border-color: #7760b4;
    }
  `}
`;
```

## <mark style="background: #FF5582A6;">버튼 모양 링크</mark>
## <mark style="background: #FFB86CA6;">as</mark>

Styled component에 `as` attribute를 사용하면 value인 태그 이름으로 element가 변경된다.

```jsx
// Button
const Button = styled.button`
  /* ... */
`;

// Main
<Button href="https://example.com" as="a"> // a tag로 변경
  LinkButton
</Button>
```

## <mark style="background: #FF5582A6;">Transient  Prop</mark>

아무리 JSX여도 html을 return하기 때문에 잘못된 attribute를 주입하게 되면 오류가 발생한다.

예를 들어 보면

```jsx
import styled from 'styled-components';

function Link({ className, children, ...props }) {
  return (
    <a {...props} className={className}>
      {children}
    </a>
  );
};

const StyledLink = styled(Link)`
  text-decoration: ${({ underline }) => underline ? `underline` : `none`};
`;

function App() {
  return (
    // underline이라는 prop을 내리고 이 prop은 Link로 가서 a에 그대로 실리게 된다. 오류!!!
    <StyledLink underline={false} href="https://codeit.kr">  
      Codeit으로 가기
    </StyledLink>
  );
}

export default App;
```

위의 코드는 underline이라는 attribute에 문자열이 아닌 false를 넣어서 오류가 뜨게 된다.

attribute는 html이기 때문에 문자열이여야 한다.

이를 방지하기 위해서 다음과 같은 방법을 쓸 수도 있지만

```jsx
function Link({ className, children, underline, ...props }) { 
// underline을 따로 빼서 props를 넣을 때 오류 발생 안되게 함.
  return (
    <a {...props} className={className}>
      {children}
    </a>
  );
};
```

이는 근본적인 문제를 해결하진 못한다. (이후에도 계속 추가해야함.)

Transient(일시적인) Prop(`$`)을 사용하면 styled로 인해서 상속받은 컴포넌트에는 해당 prop이 안 넘어가게 된다.

```jsx
import styled from 'styled-components';

function Link({ className, children, ...props }) {
  return (
    <a {...props} className={className}>
      {children}
    </a>
  );
};

const StyledLink = styled(Link)`
  text-decoration: ${({ $underline }) => $underline ? `underline` : `none`};
`;

function App() {
  return (
    // $을 이용해서 transient prop을 사용했다. 이제 Link로 underline이 넘어가지 않는다.
    <StyledLink $underline={false} href="https://codeit.kr"> 
      Codeit으로 가기
    </StyledLink>
  );
}

export default App;
```

## <mark style="background: #FF5582A6;">코드에서 배운거</mark>

styled.div 처럼 확장성 없게 짜는 것보단 Component 하나로 UI를 구성하고 styled() 함수로 css를 더해가는 방식이 구조상으로 깔끔한 듯

```jsx
import styled from 'styled-components';
import Spinner from './Spinner';

function BaseButton({ loading, children, ...props }) {
  return (
    <button {...props}>{loading ? <Spinner /> : children}</button>
  );
}

// 아래 코드에서 $round는 transient prop이라서 BaseButton까지 전달되지 않는다.
// 그래서 false, true가 value여도 괜찮음.
const Button = styled(BaseButton)`
  background-color: #6500c3;
  border: none;
  border-radius: ${({ $round }) => ($round ? `9999px` : `8px`)};
  color: #ffffff;
  cursor: pointer;
  font-size: 18px;
  padding: 16px;

  &:hover,
  &:active {
    background-color: #7760b4;
  }
`;

export default Button;
```

## <mark style="background: #FF5582A6;">Styled Components의 원리</mark>

## <mark style="background: #FFB86CA6;">Tagged Function</mark>

```jsx
function h1(strings, ...values) {
  return [strings, values];
}
const result = h1`color: pink;`;
console.log(result); // [['color: pink;'], []]
```

JS에서는 함수를 선언하고 backtick으로 파라미터를 담아서 호출할 수 있다.

```jsx
function h1(strings, ...values) {
  return [strings, values];
}
const backgroundColor = 'black';
const result2 = h1`
  background-color: ${backgroundColor};
  color: ${"red"};
  font-size: 16px;
  font-weight : ${"400px"};
`;
console.log(result2);
[['background-color:','color:','font-size: 16px;font-weight:'],['black','red','400px']]
```
![[Pasted image 20240420161643.png]]

${}을 사용해서 만약 한 attribute의 끝이 : 로 끝나면 해당 ${}안의 값은 두번째 배열로 가고, 다음 attribute으로 다시 

시작하게 되어 결국 첫번째 배열을 쭉 넣다가 두번째 배열 인덱스가 존재하면 해당 인덱스도 뒤에 붙여서 같이 넣

는 식으로 구현하면 하나의 css string이 탄생한다.

```jsx
function h1(strings, ...values) {

// 리턴할 Component를 만든다. children을 받아야 외부에서 적은걸 가져올 수 있다.
  function Component({ children,...props }) {

// values가 존재하면 해당 인덱스의 strings요소가 ${}로 끝나서 color: 처럼 :로 끝났다는 거임 
// 해당 인덱스의 values를 뒤에 붙혀 줘야함.
    let style = '';
    for (let i = 0; i < strings.length; ++i) {
      style += strings[i];
      
// 삽입된 값이 함수이면 props 객체를 함수에 담아서 실행하고 style에 담는다.
      if (typeof values[i] === 'function') {
        const fn = values[i];
        style += fn(props);

// 함수가 아니면 ${}에 함수말고 다른 것을 담은 것, strings뒤에 붙히듯 style에 그대로 담는다.
      } else if (values[i]) {
        style += values[i];
      }
    }

// 랜덤으로 이름을 짓는다. (Styled Component의 방식)
    const className = `my-sc-${style.length}`;

// `<style>` 태그로 만든 CSS 코드를 렌더링한다
    return (
// style을 선언하면서 h1에도 해당 클래스를 주입한다.
	<>
        <style>{`.${className} {${style}}`}</style>
        <h1 className={className}>{children}</h1>
	</>
    );
  }

// Component 리턴
  return Component;
}

const backgroundColor = 'black';
// 리터럴 문자열 방식으로 호출
const StyledH1 = h1`
  color: pink;
  ${({ dark }) => dark && 'background-color: black;'}
`;

function App() {
  return <StyledH1 dark>Hello World</StyledH1>;
}

export default App;
```

위 같은 느낌으로 Styled Component가 구현되었을 것이다.

App에서 dark라는 props를 받아서 StyledH1에서 사용하는데 이건 함수라서 typeof values[i] === 'function'에 걸려

서 함수에다가 dark를 담은 리턴값이 들어가고 등등 JS로 설계 가능한 구조인 것