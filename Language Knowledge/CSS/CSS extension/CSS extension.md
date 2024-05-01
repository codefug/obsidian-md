SCSS 포함 블로그 링크
https://velog.io/@codefug/BEM-SCSS-%EB%AF%B8%EB%9F%89-%EC%B2%A8%EA%B0%80

![[CSS 방법론_240320_160845_3.jpg]]![[CSS 방법론_240320_160845_4.jpg]]![[CSS 방법론_240320_160845_4.jpg]]![[CSS 방법론_240320_160845_5.jpg]]

## SCSS

## 문법

## Nesting

하위 css로 만들 수 있다.

```scss
nav {                     // nav ul {...}
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

## Mixin

@mixin 으로 뭉치를 만들고 @include로 사용할 수 있다

```scss
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, .25);
  color: #fff;
}

.info {
  @include theme;
}

.alert {
  @include theme($theme: DarkRed);
}

.success {
  @include theme($theme: DarkGreen);
}
```

## Variables

$을 이용해서 변수로 만들 수 있다.

```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

## BEM 활용

```jsx
block {
	&__element{
		&--modifier{
		
		}
	}
}
```

BEM을 이런식으로 구성할 수 있다.

## vscode extension

SCSS를 자동으로 컴파일링해주는 watcher가 있었는데 react에서 scss를 import할 수 있게 해주기 때문에 필요 없어짐

## CSS Modules

![[Pasted image 20240422164640.png]]

서로 다른 파일에 같은 클래스 이름을 동시에 꾸밀 경우 문제가 발생할 수 있는데 React에서는 클래스 이름들을 프로퍼티로 가지고 있어서 따로 꺼낼 수 있다.

```jsx
// Homepage.module.css
.logo { /* ... */ }

.sections { /* ... */ }

// Sidebar.module.css
.logo { /* ... */ }

.searchInput { /* ... */ }

.menu { /* ... */ }

// Sidebar.jsx

import styles from './Sidebar.module.css';

export default function Sidebar() {
  return (
    <div>
      <img className={styles.logo} src="/logo.svg" />
      <input className={styles.searchInput} />
      <nav className={styles.menu}>
        ...
      </nav>
    </div>
  );
}
```

## Vite에서 module css

`.module.css`로 끝나는 파일은 전부 CSS module 파일로 취급하고 이걸 리턴하면 module object를 리턴한다.

```jsx
// somefile.module.css
.red {
  color: red;
}

// somefile.jsx
import classes from './example.module.css'
document.getElementById('foo').className = classes.red
```

css.modules option에서 configured할 수 있는데 

css.modules.localsConvention에서 camelCase를 풀어주면 named import를 사용할 수 있다.

```jsx
// .apply-color -> applyColor
import { applyColor } from './example.module.css'

document.getElementById('foo').className = applyColor
```

## CSS-in-JS

[[FeTech/Styled Components/Codeit/Styled Components]]

## Tailwind CSS

[[FeTech/Tailwind CSS/Codeit/Tailwind CSS]]

