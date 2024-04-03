![[CSS Responsive web_240321_105709_1 (3).jpg]]
## <mark style="background: #FFB86CA6;">사용하는 이유</mark>

1. @media와 @import 로 style을 조건에 맞춰서 입히고 싶다.
2. `<style>`,`<link>`,`<source>`에 대한 특정 media 나 다른 `media=`, `sizes=` 속성이 있는 HTML element를 target
3. media states를 모니터링 하기 위해서 (Window.matchMedia(), EventTarget.addEventListener()) 

## <mark style="background: #FFB86CA6;">@media</mark>

## <mark style="background: #FFF3A3A6;">사용방법</mark>

@media 가 코드의 가장 위에 있어야 한다
```css
/* At the top level of your code */
@media screen and (min-width: 900px) {
  article {
    padding: 1rem 3rem;
  }
}

/* Nested within another conditional at-rule */
@supports (display: flex) {
  @media screen and (min-width: 900px) {
    article {
      display: flex;
    }
  }
}
```



## <mark style="background: #FFB86CA6;">문법</mark>

| media 구성 요소    | 설명                                                              | 예시                                |
| -------------- | --------------------------------------------------------------- | --------------------------------- |
| media types    | media query가 적용될 큰 카테고리를 의미한다.                                  | `all`(default) ,`print`, `screen` |
| media features | 웹 브라우저 같은 user agent, output device, environment 의 특정 특징을 나타낸다. |                                   |

1. 선택적인 Media Type과 수많은 Media feature들로 이루어져 있다.

2. case-insensitive임.

3. logical operaotr로 묶을 수 있다. (ex `and` `not` `only`) 

4. `,`이걸로 여러 조건에 한 특징 적용 가능

5. Media Query는 document가 display 되면서 media feature expression에 부합하면 true로 계산한다. 
   (모르는 media type이 있는 query는 false이다.)
   (query가 false더라도 Media query가 있는 style sheet는 우선순위에서 밀리긴 해도 다운로드 된다, 적용은 안됨, 다운로드하는 건 뭔 이유가 있나봄)

## <mark style="background: #FFF3A3A6;">media type 타겟</mark>

특정 디바이스를 타겟할 수 있다. 

ex) printer기, screen
```css
@media print,screen {
	/* ... */
}
```

다 deprecated되고 print, screen, all만 남음.
## <mark style="background: #FFF3A3A6;">media feature 타겟</mark>

1. user-agent의 특징에 따라서 다르게 적용하기 위해 필요하다.
	ex) hover할 수 있는 디바이스 일 경우
	```css
	@media (hover: hover){
	/*... */
	}
	```

2. 보통 range feature 혹은 discrete feature(극과 극)이다. 
   아래 예시는 discrete 특징인 orientation이다. (`landscape` or `portrait`)
	```css
	@media print and (orientation: portrait){
		/* */
	}
	```
	아래 예시는 range feature인 max-width이다. 보통 range feature는 max-, min-을 prefix로 갖는다.
	```css
	@media (max-width: 1250px) {
		/*  …  */
	}
	
	@media (width < = 1250px){
		/* ... */
	}
	
	@media (min-width: 30em) and (max-width: 50em) {
		  /* … */
	}

	@media (30em < width < 50em) {
		  /* … */
	}
	```

3. value 없이 하면 feature value가 0이나 none이 아니라면 nested style이 사용된다.
	아래 예시는 color screen을 가진 device면 다 할당되는 CSS코드
	```css
	@media (color){
	/*...*/
	}
	```

## <mark style="background: #FFF3A3A6;">복잡한 query 생성</mark>

## <mark style="background: #BBFABBA6;">logical operator</mark>

| logical operator | 설명                                   |
| ---------------- | ------------------------------------ |
| `not`            | media query의 일반적인 의미를 거꾸로 한다.        |
| `and`            | 여러 media feature, media type를 합침     |
| `only`           | 옛날 browser들이 style을 입히는 걸 방지한다.      |
| `,`              | 서로 다른 상황에 같은 동작을 하게 함.               |
| `or`             | 하나라도 media feature, media type에 통과되면 |

## <mark style="background: #ABF7F7A6;">and</mark>

```CSS
@media screen and (min-width: 30em) and (orientation: landscape) {
  /* … */
}
```

## <mark style="background: #ABF7F7A6;">not</mark>

```css
@media not screen and (color), print and (color) {
  /* … */
}
//아래와 같다.
@media (not (screen and (color))), print and (color) {
  /* … */
}
```
single query 하나에 not전체가 적용된다. (,는 다음 쿼리로 이동하는 거다.)

## <mark style="background: #ABF7F7A6;">or</mark>

```css
@media (not (color)) or (hover) {
  /* … */
}
```


