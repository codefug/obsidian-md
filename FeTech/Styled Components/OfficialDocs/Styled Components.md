styled-component가 있는 이유

1. Automatic critical CSS : 컴포넌트를 추적하고 style을 주입한다.
2. No class name bugs : classname이 필요 없다.
3. Easier deletion of CSS : 모든 스타일이 component에 묶여 있기 때문에 component가 안쓰이고 삭제되면 style은 삭제된다.
4. Simple dynamic styling : dynamic styling을 prop이나 theme으로 받아서 처리할 수 있게 된다.
5. Painless maintenance : 스타일을 찾으려고 뒤지지 않아도 된다.
6. Automatic vendor prefixing : css를 표준에 맞게 작성하고 나머지는 자동

## <mark style="background: #FF5582A6;">설치</mark>

`npm install styled-components`

## <mark style="background: #FF5582A6;">특징</mark>

1. tagged template literal을 사용한다.
2. component와 style 사이의 mapping을 없앤다.

```jsx
// Create a Title component that'll render an <h1> tag with some styles
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: #BF4F74;
`;

// Create a Wrapper component that'll render a <section> tag with some styles
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// Use Title and Wrapper like any other React component – except they're styled!
render(
  <Wrapper>
    <Title>
      Hello World!
    </Title>
  </Wrapper>
);
```
![[Pasted image 20240425185418.png]]

## <mark style="background: #FF5582A6;">Adapting based on props</mark>

props을 기준으로 style을 지정할 수 있다.

```jsx
const Button = styled.button<{ $primary?: boolean; }>`
  /* Adapt the colors based on primary prop */
  background: ${props => props.$primary ? "#BF4F74" : "white"};
  color: ${props => props.$primary ? "white" : "#BF4F74"};

  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;
`;

render(
  <div>
    <Button>Normal</Button>
    <Button $primary>Primary</Button>
  </div>
);
```
![[Pasted image 20240425185638.png]]

## <mark style="background: #FF5582A6;">Extending Styles</mark>

`styled()` constructor를 이용해서 styled component를 감싸면 상속할 수 있다.

```jsx
// The Button from the last section without the interpolations
const Button = styled.button`
  color: #BF4F74;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;
`;

// A new component based on Button, but with some override styles
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <TomatoButton>Tomato Button</TomatoButton>
  </div>
);
```
![[Pasted image 20240425190027.png]]
style은 똑같은데 component나 tag만 바꾸고 싶을때는 `as`를 사용한다.

```jsx
const Button = styled.button`
  display: inline-block;
  color: #BF4F74;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;
  display: block;
`;

const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

render(
  <div>
    <Button>Normal Button</Button>
    <Button as="a" href="#">Link with Button styles</Button>
    <TomatoButton as="a" href="#">Link with Tomato Button styles</TomatoButton>
  </div>
);
```
![[Pasted image 20240425200627.png]]
Custom element에도 적용할 수 있다.

```jsx
const Button = styled.button`
  display: inline-block;
  color: #BF4F74;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;
  display: block;
`;

const ReversedButton = props => <Button {...props} children={props.children.split('').reverse()} />

render(
  <div>
    <Button>Normal Button</Button>
    <Button as={ReversedButton}>Custom Button with Normal Button styles</Button>
  </div>
);
```
![[Pasted image 20240425200717.png]]

## <mark style="background: #FFB86CA6;">Styling any component</mark>

className prop만 갖고 있다면 third-party component에도 `styled`를 이용해서 styling을 할 수 있다.

```jsx
// This could be react-router-dom's Link for example
const Link = ({ className, children }) => (
  <a className={className}>
    {children}
  </a>
);

const StyledLink = styled(Link)`
  color: #BF4F74;
  font-weight: bold;
`;

render(
  <div>
    <Link>Unstyled, boring Link</Link>
    <br />
    <StyledLink>Styled, exciting Link</StyledLink>
  </div>
);
```
![[Pasted image 20240425201123.png]]
styled.tagname은 사실 styled("tagname")과 똑같이 동작하는 것이다.

## <mark style="background: #FF5582A6;">Passed props</mark>

HTML element를 직접 렌더링하는 styled component의 경우 prop으로 attribute를 전달할 수 있으며
(비표준은 알아서 거름)

Component의 경우 전부 prop 처리 된다.

```jsx
// Create an Input component that'll render an <input> tag with some styles
const Input = styled.input<{ $inputColor?: string; }>`
  padding: 0.5em;
  margin: 0.5em;
  color: ${props => props.$inputColor || "#BF4F74"};
  background: papayawhip;
  border: none;
  border-radius: 3px;
`;

// Render a styled text input with the standard input color, and one with a custom input color
render(
  <div>
    <Input defaultValue="@probablyup" type="text" /> // text와 defaultValue를 attribute로 받는다.
    <Input defaultValue="@geelen" type="text" $inputColor="rebeccapurple" />
  </div>
);
```
![[Pasted image 20240425201546.png]]

## <mark style="background: #FF5582A6;">Coming from CSS</mark>

1. styled Component는 element와 그걸 styling하는 rule의 combination이다.
2. render method 밖에서 styled component를 선언해야 한다.
	1. rendering마다 재생성되게 된다.
	2. caching을 방해하고 rendering 속도를 낮춘다.

```jsx
const StyledWrapper = styled.div`
  /* ... */
`;
const Wrapper = ({ message }) => {
  return <StyledWrapper>{message}</StyledWrapper>;
};
```

[[https://medium.com/building-crowdriff/styled-components-to-use-or-not-to-use-a6bb4a7ffc21|추천 포스트]]

## <mark style="background: #FFB86CA6;">Pseudoelements, pseudoselectors, and nesting</mark>

styled-components는 내부적으로 stylis라는 preprocessor를 사용한다.
## <mark style="background: #FFF3A3A6;">&</mark>

component의 모든 instance를 의미한다. 광범위한 override에 사용된다.

```jsx
const Thing = styled.div.attrs((/* props */) => ({ tabIndex: 0 }))`
  color: blue;

  &:hover {
    color: red; // <Thing> when hovered
  }

  & ~ & {
    background: tomato; // <Thing> as a sibling of <Thing>, but maybe not directly next to it
  }

  & + & {
    background: lime; // <Thing> next to <Thing>
  }

  &.something {
    background: orange; // <Thing> tagged with an additional CSS class ".something"
  }

  .something-else & {
    border: 1px solid; // <Thing> inside another element labeled ".something-else"
  }
`

render(
  <React.Fragment>
    <Thing>Hello world!</Thing>
    <Thing>How ya doing?</Thing>
    <Thing className="something">The sun is shining...</Thing>
    <div>Pretty nice day today.</div>
    <Thing>Don't you think?</Thing>
    <div className="something-else">
      <Thing>Splendid.</Thing>
    </div>
  </React.Fragment>
)
```
![[Pasted image 20240425203131.png]]
## <mark style="background: #FFF3A3A6;">&&</mark>

component의 instance 하나를 지칭한다. conditional styling overrides시에 유용하다.

```jsx
const Input = styled.input.attrs({ type: "checkbox" })``;

const Label = styled.label`
  align-items: center;
  display: flex;
  gap: 8px;
  margin-bottom: 8px;
`

const LabelText = styled.span`
  ${(props) => {
    switch (props.$mode) {
      case "dark":
        return css`
          background-color: black;
          color: white;
          ${Input}:checked + &&{
            color: blue;
          }
        `;
      default:
        return css`
          background-color: white;
          color: black;
          ${Input}:checked + && {
            color: red;
          }
        `;
    }
  }}
`;

render(
  <React.Fragment>
    <Label>
      <Input defaultChecked />
      <LabelText>Foo</LabelText>
    </Label>
    <Label>
      <Input />
      <LabelText $mode="dark">Fdaso</LabelText>
    </Label>
    <Label>
      <Input defaultChecked />
      <LabelText>Foo</LabelText>
    </Label>
    <Label>
      <Input defaultChecked />
      <LabelText $mode="dark">Foo</LabelText>
    </Label>
  </React.Fragment>
)
```
![[Pasted image 20240425203813.png]]

그냥 `&&` 만 사용하면 precedence boost이다. conflicting style이 있을 때 무시하고 이것 먼저 적용한다.

```jsx
const Thing = styled.div`
   && {
     color: blue;
   }
 `

 const GlobalStyle = createGlobalStyle`
   div${Thing} {
     color: red;
   }
 `

 render(
   <React.Fragment>
     <GlobalStyle />
     <Thing>
       I'm blue, da ba dee da ba daa
     </Thing>
   </React.Fragment>
 )
```
![[Pasted image 20240425204107.png]]

## <mark style="background: #FFF3A3A6;">no ampersand</mark>

ampersand 없이는 그냥 children selector로 취급된다.

```jsx
const Thing = styled.div`
  color: blue;

  .something {
    border: 1px solid; // an element labeled ".something" inside <Thing>
    display: block;
  }
`

render(
  <Thing>
    <label htmlFor="foo-button" className="something">Mystery button</label>
    <button id="foo-button">What do I do?</button>
  </Thing>
)
```
![[Pasted image 20240425204203.png]]

## <mark style="background: #FFB86CA6;">Attaching additional props</mark>

prop만 전달하는 불필요한 wrapper를 피하기 위해서 `.attrs` constructor를 사용한다. 
추가적인 props를 component에 추가한다.
(static props, third-party prop(`activeClassName`))

```jsx
const Input = styled.input.attrs<{ $size?: string; }>(props => ({
  
  // we can define static props
  type: "text", // text type으로 HTML에 그대로 attribute로써 내려감

  // or we can define dynamic ones
  $size: props.$size || "1em", //기본 값이 1em

}))`
  color: #BF4F74;
  font-size: 1em;
  border: 2px solid #BF4F74;
  border-radius: 3px;

  /* here we use the dynamically computed prop */
  margin: ${props => props.$size};
  padding: ${props => props.$size};
`;

render(
  <div>
    <Input placeholder="A small text input" />
    <br />
    <Input placeholder="A bigger text input" $size="2em" />
  </div>
);
```
![[Pasted image 20240425210107.png]]

## <mark style="background: #FFF3A3A6;">Overriding `.attrs`</mark>

wrapping될 경우 `.attrs`는 안에서 밖으로 적용된다.

이는 일반 CSS의 순서 규칙과 같다.

```jsx
const Input = styled.input.attrs<{ $size?: string }>((props) => ({
	type: 'text',
	$size: props.$size || '1em',
}))`
	border: 2px solid #bf4f74;
	margin: ${(props) => props.$size};
	padding: ${(props) => props.$size};
`;

// Input's attrs will be applied first, and then this attrs obj
const PasswordInput = styled(Input).attrs({
  type: "password",
})`
  // similarly, border will override Input's border
  border: 2px solid aqua;
`;

render(
  <div>
    <Input placeholder="A bigger text input" $size="2em" />
    <br />
    {/* Notice we can still use the size attr from Input */}
    <PasswordInput placeholder="A bigger password input" $size="2em" />
  </div>
);
```
![[Pasted image 20240425210705.png]]

## <mark style="background: #FF5582A6;">Animations</mark>

CSS의 `@keyframes`는 한 컴포넌트에 제한되지 않아서 name collision을 일으킬 수 있다.

그래서 styled-components에서는 `keyframes` 라는 helper를 export한다.
(unique instance)

```jsx
/ Create the keyframes
const rotate = keyframes`
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
`;

// Here we create a component that will rotate everything we pass in over two seconds
const Rotate = styled.div`
  display: inline-block;
  animation: ${rotate} 2s linear infinite;
  padding: 2rem 1rem;
  font-size: 1.2rem;
`;

render(
  <Rotate>&lt; 💅🏾 &gt;</Rotate>
);
```
![[Pasted image 20240425210947.png]] 계속 돌아가는 사진 (오른쪽)

keyframes는 lazily injected된다. 그래서 code-split이 가능하고 css helper를 사용해야 한다.

```jsx
const rotate = keyframes``
// ❌ This will throw an error!
const styles = `
  animation: ${rotate} 2s linear infinite;
`
// ✅ This will work as intended
const styles = css`
  animation: ${rotate} 2s linear infinite;
`
```

v3까지는 css없이 썼음

## <mark style="background: #FF5582A6;">심화</mark>

## <mark style="background: #FFB86CA6;">Theming</mark>

`<ThemeProvider>`를 이용해서 theming을 지원한다.

context API를 이용해서 React components에 theme을 제공한다.

```jsx
/ Define our button, but with the use of props.theme this time
const Button = styled.button`
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border-radius: 3px;

  /* Color the border and text with theme.main */
  color: ${props => props.theme.main};
  border: 2px solid ${props => props.theme.main};
`;

// We are passing a default theme for Buttons that arent wrapped in the ThemeProvider
Button.defaultProps = { // 이렇게하면 기본 props에 들어가나 봄
  theme: {
    main: "#BF4F74"
  }
}

// Define what props.theme will look like
const theme = {
  main: "mediumseagreen"
};

render(
  <div>
    <Button>Normal</Button>

    <ThemeProvider theme={theme}>
      <Button>Themed</Button>
    </ThemeProvider>
  </div>
);
```
![[Pasted image 20240425212006.png]]

## <mark style="background: #FFF3A3A6;">Function themes</mark>

theme prop에 funciton을 넣을 수 있다. 이 function은 상위 ThemeProvider에서 온 부모 theme 객체를 arg로 받는다.

```jsx
/ Define our button, but with the use of props.theme this time
const Button = styled.button`
  color: ${props => props.theme.fg};
  border: 2px solid ${props => props.theme.fg};
  background: ${props => props.theme.bg};
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border-radius: 3px;
`;

// Define our `fg` and `bg` on the theme
const theme = {
  fg: "#BF4F74",
  bg: "white"
};

// This theme swaps `fg` and `bg`
const invertTheme = ({ fg, bg }) => ({
  fg: bg,
  bg: fg
});

render(
  <ThemeProvider theme={theme}>
    <div>
      <Button>Default Theme</Button>
      <ThemeProvider theme={invertTheme}>
        <Button>Inverted Theme</Button>
      </ThemeProvider>
    </div>
  </ThemeProvider>
);
```
![[Pasted image 20240425212718.png]]

## <mark style="background: #FFF3A3A6;">Getting the theme without styled components</mark>

그냥 `theme` prop으로 내리는 방식이 아닌 다른 방식이 세가지 정도 있는데

그중 최신은 v5에 나온 `useTheme`이다.

```jsx
import { useTheme } from 'styled-components'
const MyComponent = () => {
  const theme = useTheme()
  console.log('Current theme: ', theme)
  // ...
}
```

바로 최근의 theme을 가져온다.

## <mark style="background: #FFF3A3A6;">The `theme` prop</mark>

theme prop에다가 다른것 넣어서 override 가능

```jsx
// Define our button
const Button = styled.button`
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border-radius: 3px;

  /* Color the border and text with theme.main */
  color: ${props => props.theme.main};
  border: 2px solid ${props => props.theme.main};
`;

// Define what main theme will look like
const theme = {
  main: "mediumseagreen"
};

render(
  <div>
    <Button theme={{ main: "royalblue" }}>Ad hoc theme</Button>
    <ThemeProvider theme={theme}>
      <div>
        <Button>Themed</Button>
        <Button theme={{ main: "darkorange" }}>Overridden</Button>
      </div>
    </ThemeProvider>
  </div>
);
```
![[Pasted image 20240425213353.png]]

## <mark style="background: #FFF3A3A6;">Refs</mark>

ref prop을 전달하면 styled target에 따라 두가지 중 하나를 전달한다.

1. underlying DOM node (`styled.div`)
2. React component instance (`React.Component`)

```jsx
const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  color: #BF4F74;
  background: papayawhip;
  border: none;
  border-radius: 3px;
`;

class Form extends React.Component {
  constructor(props) {
    super(props);
    this.inputRef = React.createRef();
  }

  render() {
    return (
      <Input
        ref={this.inputRef}
        placeholder="Hover to focus!"
        onMouseEnter={() => {
          this.inputRef.current.focus()
        }}
      />
    );
  }
}

render(
  <Form />
);
```
onMouseEnter시 focus된다. 근데 useRef로 대신할 수 있을듯

## <mark style="background: #FFB86CA6;">Security</mark>

user input을 style로 지정하면 attacker가 CSS injection공격을 할 수 있다.

CSS sanitization 하는 법 [[https://github.com/mathiasbynens/CSS.escape|css.escape polyfil]]

CSS.escape가 있긴 한데 모든 브라우저 지원은 안됨

## <mark style="background: #FFB86CA6;">Existing CSS</mark>

sc는 stylesheet를 실제로 만들어서 해당 클래스를 className을 통해서 붙힌다.

런타임 시 document의 head 끝에 stylesheet를 주입한다.

## <mark style="background: #FFB86CA6;">Styling normal React Components</mark>

`styled(MyComponent)` 이 활용될려면 passed-in `className`이 필요하다.

이미 존재하는 className이 있다면 하나 더 추가해놓으면 된다.

```jsx
class MyComponent extends React.Component {
  render() {
    // Attach the passed-in className to the DOM node
    return <div className={`some-global-class ${this.props.className}`} />
  }
}
```

## <mark style="background: #FFB86CA6;">Specificity issues</mark>

sc는 런타임 시 document head 끝에 주입되기 때문에 specificity가 다른 className styling에 비해서 높다. 이를 해결하기 위해서는 specificity를 높혀야 한다.

```jsx
/* my-component.css */
.red-bg.red-bg {
  background-color: red;
}
```

## <mark style="background: #FFB86CA6;">Avoiding conflicts with third-party styles and scripts</mark>

host page가 아닌 fully control못하는 곳에 deploy할 경우 host page의 style과 충돌하지 않기 위해서 다음과 같이 할 수 있다.

```css
body.my-body button {
  padding: 24px;
}
```
specificity를 높혀서 충돌 가능성을 줄인다.

```jsx
styled.button`
  padding: 16px;
`
```

완벽하진 않지만 [[https://github.com/QuickBase/babel-plugin-styled-components-css-namespace|babel]] 과 함께 써서 styled component의 specificity를 늘릴 수 있다.

babel에서는 모든 styled component의 CSS namespace를 제공한다. 여기서 #~ 이렇게 id를 적어버리면 specificity가 확 올라가서 예방이 크게 된다.

같은 sc끼리 충돌나면 `process.env.SC_ATTR`을 code bundle에 정의하면 된다. style tag attribute,`data-styled`를 override하는데 이건 각 sc instance가 고유의 tag를 갖게 한다.

## <mark style="background: #FFB86CA6;">Tagged Template Literals</mark>

interpolations가 없다면 첫 arg는 string이 담긴 array로 담긴다.
```jsx
// These are equivalent:
fn`some string here`;
fn(['some string here']);
```

interpolations이 통과하면 array는 interpolation을 기준으로 나눠져서 passed string을 담고, 나머지는 interpolations이 된다. 순서대로

```jsx
const aVar = 'good';
// These are equivalent:
fn`this is a ${aVar} day`;
fn(['this is a ', ' day'], aVar);
```

sc에서는 `undefined`, `null`, `false`, `""`을 무시하기 때문에 short-circuit evaluation(`&&`)을 사용해도 된다.

```jsx
const Title = styled.h1<{ $upsideDown?: boolean; }>`
  /* Text centering won't break if props.$upsideDown is falsy */
  ${props => props.$upsideDown && 'transform: rotate(180deg);'}
  text-align: center;
`;
```

[[https://mxstbr.blog/2016/11/styled-components-magic-explained/|추천 사이트]]

## <mark style="background: #FFB86CA6;">SSR</mark>

나중에 하셈

## <mark style="background: #FFB86CA6;">Referring to other components</mark>

context 재정의를 component styling에 하는 여러가지 방법이 있다. 

interpolation에 css selector paradigm을 집어넣는거 아니면 하기 쉽지 않다.

sc는 component selector로 이걸 해결했다. `styled()` factory function으로 묶인 component는 CSS Class에도 할당된다.

```jsx
const Link = styled.a`
  display: flex;
  align-items: center;
  padding: 5px 10px;
  background: papayawhip;
  color: #BF4F74;
`;

const Icon = styled.svg`
  flex: none;
  transition: fill 0.25s;
  width: 48px;
  height: 48px;

  ${Link}:hover & {                    // component를 담음
    fill: rebeccapurple;
  }
`;

const Label = styled.span`
  display: flex;
  align-items: center;
  line-height: 1.2;

  &::before {
    content: '◀';
    margin: 0 10px;
  }
`;

render(
  <Link href="#">
    <Icon viewBox="0 0 20 20">
      <path d="M10 15h8c1 0 2-1 2-2V3c0-1-1-2-2-2H2C1 1 0 2 0 3v10c0 1 1 2 2 2h4v4l4-4zM5 7h2v2H5V7zm4 0h2v2H9V7zm4 0h2v2h-2V7z"/>
    </Icon>
    <Label>Hovering my parent changes my style!</Label>
  </Link>
);
```
![[Pasted image 20240426005451.png]]

## <mark style="background: #FFB86CA6;">Caveat</mark>

Class component 내용

## <mark style="background: #FFB86CA6;">Style Objects</mark>

sc에서는 Object로도 CSS를 작성할 수 있다.  기존 style object 에서 styled-components로 옮기는 과정에서 유용하다.

```jsx
// Static object
const Box = styled.div<{ $background?: string; }>({
  background: '#BF4F74',
  height: '50px',
  width: '50px'
});

// Adapting based on props
const PropsBox = styled.div(props => ({
  background: props.$background,
  height: '50px',
  width: '50px'
}));

render(
  <div>
    <Box />
    <PropsBox $background="blue" />
  </div>
);
```