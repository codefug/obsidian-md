## <mark style="background: #FF5582A6;">Vite에서 Tailwind 설치</mark>

1. Vite 설치
```terminal
npm create vite@latest my-project -- --template react
cd my-project
```
2. Tailwind CSS 설치
```terminal
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```
3. tailwind.config.js에서 모든 template file의 경로 업로드
```js
//tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
4. `./src/index.css` 파일에 각 Tailwind layer에 대한 @tailwind directives 추가
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```
5. build 시작
```termial
npm run dev
```
6. Tailwind 사용
```jsx
export default function App() {
  return (
    <h1 className="text-3xl font-bold underline">
      Hello world!
    </h1>
  )
}
```

## <mark style="background: #FF5582A6;">Utility-First Fundamentals</mark>

이미 존재하는 클래스로 앱을 만들자.
![[Pasted image 20240422172410.png]]
```html
<div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-lg flex items-center space-x-4">
  <div class="shrink-0">
    <img class="h-12 w-12" src="/img/logo.svg" alt="ChitChat Logo">
  </div>
  <div>
    <div class="text-xl font-medium text-black">ChitChat</div>
    <p class="text-slate-500">You have a new message!</p>
  </div>
</div>
```

| categories | utility          | classname      |
| ---------- | ---------------- | -------------- |
| Layout     | margin           | mx-auto        |
|            | padding          | p-6            |
|            | flexbox          | flex, shrink-0 |
|            | space-between    | space-x-4      |
|            | max-width        | max-w-sm       |
|            | width            | w-12           |
|            | height           | h-12           |
| appearance | background-color | bg-white       |
|            | border radius    | rounded-xl     |
|            | box-shadow       | shadow-lg      |
|            | font size        | text-xl        |
|            | text color       | text-black     |
|            | font-weight      | font-medium    |

## <mark style="background: #FFB86CA6;">장점</mark>

1. Class Name을 신경 쓸 필요가 없다.
2. CSS 파일이 늘어나지 않는다.
3. CSS를 변경할 때 다른 HTML들을 신경쓰지 않아도 된다.

## <mark style="background: #FFB86CA6;">inline style과 다른 점</mark>

1. 제약 조건이 있는 디자인
미리 적용된 design system에서 style을 선택해서 사용해서 일관된 UI를 제공한다.
2. Responsive design
responsive utilities를 통해서 완벽한 responsive interface를 만들 수 있다.
3. hover, focus and other state
Tailwind의 state variants를 통해서 utility class로 state를 style할 수 있다.

## <mark style="background: #FFB86CA6;">maintainability concerns</mark>

유지보수성에서 가장 큰 문제는 반복되는 utility combination인데 이건 사실 부분이나 컴포넌트 추출과 editor, language features(multi-cursor 같은) 을 사용해서 해결할 수 있다.

## <mark style="background: #FF5582A6;">Responsive Design</mark>

responsive utility variants를 사용해보자.

Tailwind의 모든 utility class는 breakpoint에 따라서 조건에 맞춰 적용될 수 있다.

## <mark style="background: #FFB86CA6;">5가지의 break point</mark>

| Breakpoint prefix | Minimum width | CSS                                  |
| ----------------- | ------------- | ------------------------------------ |
| `sm`              | 640px         | `@media (min-width: 640px) { ... }`  |
| `md`              | 768px         | `@media (min-width: 768px) { ... }`  |
| `lg`              | 1024px        | `@media (min-width: 1024px) { ... }` |
| `xl`              | 1280px        | `@media (min-width: 1280px) { ... }` |
| `2xl`             | 1536px        | `@media (min-width: 1536px) { ... }` |

colon (`:`)을 이용해서 각 breakpoint를 지정할 수 있다.

```html
<!-- Width of 16 by default, 32 on medium screens, and 48 on large screens --> 
<img class="w-16 md:w-32 lg:w-48" src="...">
```

이 문법은 tailwindcss framework에서 모든 utility class에 적용할 수 있는 문법이다.

다음 예시는 responsive design을 적용한 예시이다.

## <mark style="background: #FFF3A3A6;">stacked layout으로 모바일 화면</mark>
![[Pasted image 20240422182817.png]]
## <mark style="background: #FFF3A3A6;">side-by-side layout으로 더 큰 화면</mark>
![[Pasted image 20240422182802.png]]
```html
<div class="max-w-md mx-auto bg-white rounded-xl shadow-md overflow-hidden md:max-w-2xl">
  <div class="md:flex">
    <div class="md:shrink-0">
      <img class="h-48 w-full object-cover md:h-full md:w-48" src="/img/building.jpg" alt="Modern building architecture">
    </div>
    <div class="p-8">
      <div class="uppercase tracking-wide text-sm text-indigo-500 font-semibold">Company retreats</div>
      <a href="#" class="block mt-1 text-lg leading-tight font-medium text-black hover:underline">Incredible accommodation for your team</a>
      <p class="mt-2 text-slate-500">Looking to take your team away on a retreat to enjoy awesome food and take in some sunshine? We have a list of places to do just that.</p>
    </div>
  </div>
</div>
```

| utility      |            classname            |
| ------------ | :-----------------------------: |
| md:flex      | medium과 더 큰 화면에서 `display:flex` |
| md: shrink-0 |   medium과 더 큰 화면에서 `shrink:0`   |
| md:h-full    |  medium과 더 큰 화면에서 full height   |
| md:w-48      |  medium과 더 큰 화면에서 `width:48px`  |

## <mark style="background: #FFB86CA6;">mobile부터 먼저 작업하라</mark>

breakpoint system을 작업할 때는 mobile부터 작업하는게 편하다.

## <mark style="background: #FFF3A3A6;">Targeting mobile screens</mark>

mobile 작업을 할 때는 `sm:` 같은 것을 붙히지 않고 unprefixed version으로 작업한다. 

`sm:`은 작은 화면 대상이 아니라 `at the small breakpoint` 라고 생각해라.

640px이상일 때 된다는 거지 small screen대상이 아니다.
```html
<div class="sm:text-center"></div>
```

unprefixed version > `sm:` > `md:` 이런 순서로 작업해야 편하다.

## <mark style="background: #FFF3A3A6;">Targeting a breakpoint range</mark>

기본적으로 `md:` 같은 건 해당 breakpoint와 그 이상을 책임진다.

만약 구간을 정하고 싶으면 `md:` 같은 breakpoint하나랑 `max-*:` 에서 `*`에 breakpoint을 채운 형태로 동시에 적고 뒤에 스타일 적으면 된다.

```html
<div class="md:max-xl:flex"> 
	<!-- ... --> 
</div>
```

tailwind는 `max-*`modifier를 각 breakpoint에 생성하기 때문에 기본적으로 다음 표에 있는 것만 쓰는 것도 가능해진다.

| Modifier  | Media query                                      |
| --------- | ------------------------------------------------ |
| `max-sm`  | `@media not all and (min-width: 640px) { ... }`  |
| `max-md`  | `@media not all and (min-width: 768px) { ... }`  |
| `max-lg`  | `@media not all and (min-width: 1024px) { ... }` |
| `max-xl`  | `@media not all and (min-width: 1280px) { ... }` |
| `max-2xl` | `@media not all and (min-width: 1536px) { ... }` |

## <mark style="background: #FFF3A3A6;">Targeting a single breakpoint</mark>

만약 한 구간만 하고 싶으면 한 구간만 정해놓으면 된다.

```html
<div class="md:max-lg:flex"> 
	<!-- ... --> 
</div>
```

## <mark style="background: #FFB86CA6;">Customizing your theme</mark>

```js
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    screens: {
      'tablet': '640px',
      // => @media (min-width: 640px) { ... }

      'laptop': '1024px',
      // => @media (min-width: 1024px) { ... }

      'desktop': '1280px',
      // => @media (min-width: 1280px) { ... }
    },
  }
}
```

tailwind.config.js에서 breakpoint를 커스텀할 수 있다.

## <mark style="background: #FFB86CA6;">Arbitrary values</mark>

일회용 breakpoint를 만들고 싶다면 다음과 같이 표현하면 된다.

```html
<div class="min-[320px]:text-center max-[600px]:bg-sky-300"> 
	<!-- ... --> 
</div>
```

## <mark style="background: #FF5582A6;">Handling Hover, Focus, and Other States</mark>

Tailwind의 모든 utility class는 앞에 modifier를 더해서 condition을 만든다.

![[Pasted image 20240422230620.png]]
```html
<button class="bg-sky-500 hover:bg-sky-700 ..."> 
	Save changes 
</button>
```

Tailwind에서는 기존 class에 hover를 붙히는 기존 방식이 아닌 hover 상태에만 무언가를 하는 element에 class를 붙히는 방식이다.

기존 방식
```css
.btn-primary {
  background-color: #0ea5e9;
}
.btn-primary:hover {
  background-color: #0369a1;
}
```

Tailwindcss
```css
.bg-sky-500 { 
	background-color: #0ea5e9; 
} 
.hover\:bg-sky-700:hover { 
	background-color: #0369a1; 
}
```

Tailwind는 모든것을 포함하는 modifier를 갖고 있다.

| name                      | example                                                     |
| ------------------------- | :---------------------------------------------------------- |
| Pseudo-classes            | `:hover`, `:focus`, `:first-child`, and `:required`         |
| Pseudo-elements           | `::before`, `::after`, `::placeholder`, and `::selection`   |
| Media and feature queries | responsive breakpoints, dark mode, `prefers-reduced-motion` |
| Attribute selectors       | `[dir="rtl"]` and `[open]`                                  |

이것들을 stacked해서 구현하는 것이다.
```html
<button class="dark:md:hover:bg-fuchsia-600 ...">
  Save changes
</button>
```

## <mark style="background: #FFB86CA6;">Pseudo-classes</mark>

## <mark style="background: #FFF3A3A6;">Hover, Focus, and Active</mark>

![[Pasted image 20240422231904.png]]
```html
<button class="bg-violet-500 hover:bg-violet-600 active:bg-violet-700 focus:outline-none focus:ring focus:ring-violet-300 ...">
  Save changes
</button>
```

`:visited`, `:focus-within`, `:focus-visible` 같은 것들도 존재한다.

## <mark style="background: #FFF3A3A6;">First, last, odd, and even</mark>

first-child와 last-child를 `first`, `last` modifier로 관리한다.

![[Pasted image 20240422232330.png]]
```html
<ul role="list" class="p-6 divide-y divide-slate-200">
  {#each people as person}
    <!-- Remove top/bottom padding when first/last child -->
    <li class="flex py-4 first:pt-0 last:pb-0">
      <img class="h-10 w-10 rounded-full" src="{person.imageUrl}" alt="" />
      <div class="ml-3 overflow-hidden">
        <p class="text-sm font-medium text-slate-900">{person.name}</p>
        <p class="text-sm text-slate-500 truncate">{person.email}</p>
      </div>
    </li>
  {/each}
</ul>
```

`odd`, `even` modifier를 이용해서 짝수 홀수 자식들을 관리할 수 있다.

![[Pasted image 20240422232428.png]]
```html
<table>
  <!-- ... -->
  <tbody>
    {#each people as person}
      <!-- Use a white background for odd rows, and slate-50 for even rows -->
      <tr class="odd:bg-white even:bg-slate-50">
        <td>{person.name}</td>
        <td>{person.title}</td>
        <td>{person.email}</td>
      </tr>
    {/each}
  </tbody>
</table>
```

`:only-child`, `:first-of-type`, `:empty` 같은 structural pesudo-classes가 있다.

## <mark style="background: #FFF3A3A6;">Form states</mark>

`required`, `invalid` 그리고 `disabled` 같은 modifier를 이용해서 서로 다른 states를 관리하자.

![[Pasted image 20240422232705.png]]

```html
<form>
  <label class="block">
    <span class="block text-sm font-medium text-slate-700">Username</span>
    <!-- Using form state modifiers, the classes can be identical for every input -->
    <input type="text" value="tbone" disabled class="mt-1 block w-full px-3 py-2 bg-white border border-slate-300 rounded-md text-sm shadow-sm placeholder-slate-400
      focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500
      disabled:bg-slate-50 disabled:text-slate-500 disabled:border-slate-200 disabled:shadow-none
      invalid:border-pink-500 invalid:text-pink-600
      focus:invalid:border-pink-500 focus:invalid:ring-pink-500
    "/>
  </label>
  <!-- ... -->
</form>
```

`:read-only`, `:indeterminate`, `:chekced` 같은 다른 `pseudo-class`들이 존재한다.

## <mark style="background: #FFF3A3A6;">Styling based on parent state</mark>

부모 요소의 state를 기반으로 자식요소를 스타일링하고 싶다면 부모 요소에  `group`를 마크하고 `group-hover` 처럼 `group-*` modifier를 target element에 넣는다.

![[Pasted image 20240423094326.png]]
```html
<a href="#" class="group block max-w-xs mx-auto rounded-lg p-6 bg-white ring-1 ring-slate-900/5 shadow-lg space-y-3 hover:bg-sky-500 hover:ring-sky-500">
  <div class="flex items-center space-x-3">
    <svg class="h-6 w-6 stroke-sky-500 group-hover:stroke-white" fill="none" viewBox="0 0 24 24"><!-- ... --></svg>
    <h3 class="text-slate-900 group-hover:text-white text-sm font-semibold">New project</h3>
  </div>
  <p class="text-slate-500 group-hover:text-white text-sm">Create a new project from a variety of starting templates.</p>
</a>
```

`group-focus`, `group-active`, `group-odd` 처럼 다른 pseudo class처럼 사용하면 된다.

## <mark style="background: #BBFABBA6;">Differentiating nested groups</mark>

중첩 그룹일 경우에 특정 부모 그룹의 state에 따라서 동작하고 싶다면 부모 group을 `group/{name}`으로 적용하고 싶은 자식에다가 `group-*/{name}`(`group-hover/{name}`)을 적어넣으면 된다.

![[Pasted image 20240423095750.png]]

```html
<ul role="list">
  {#each people as person}
    <li class="group/item hover:bg-slate-100 ...">
      <img src="{person.imageUrl}" alt="" />
      <div>
        <a href="{person.url}">{person.name}</a>
        <p>{person.title}</p>
      </div>
      <a class="group/edit invisible hover:bg-slate-200 group-hover/item:visible ..." href="tel:{person.phone}">
        <span class="group-hover/edit:text-gray-700 ...">Call</span>
        <svg class="group-hover/edit:translate-x-0.5 group-hover/edit:text-slate-500 ...">
          <!-- ... -->
        </svg>
      </a>
    </li>
  {/each}
</ul>
```

## <mark style="background: #BBFABBA6;">Arbitrary groups</mark>

`group-[*]`을 이용해서 일회용 modifier를 만들 수 있다.

```html
<div class="group is-published">
  <div class="hidden group-[.is-published]:block">
    Published
  </div>
</div>
```

```css
.group.is-published .group-\[\.is-published\]\:block {
  display: block;
}
```

`[]` 는 attribute selecotr를 `&`은 자기자신을 나타낸다.

`_`은 공백을 나타내니 아래 코드는 이렇게 해석된다.

[[https://stackoverflow.com/questions/73666015/nested-brackets-and-ampersand-usage-in-tailwind-ui-examples|stackoverflow]]

```html
<div class="group">
  <div class="group-[:nth-of-type(3)_&]:block">
    <!-- ... -->
  </div>
</div>
```

```css
:nth-of-type(3) .group .group-\[\:nth-of-type\(3\)_\&\]\:block {
//3번째 group 아래에 있는 요소가 적용된다.
display: block;
}
```

## <mark style="background: #FFF3A3A6;">Styling based on sibling state</mark>

`peer` 을 sibling에 적고 `peer-*`(`peer-invalid` 같은) 을 target에 적으면 sibling에 기반해서 target의 state을 처리할 수 있다.

![[Pasted image 20240423111829.png]]
```html
<form>
  <label class="block">
    <span class="block text-sm font-medium text-slate-700">Email</span>
    <input type="email" class="peer ..."/>
    <p class="mt-2 invisible peer-invalid:visible text-pink-600 text-sm">
      Please provide a valid email address.
    </p>
  </label>
</form>
```

`peer-focus`, `peer-required`, `peer-disabled` 같은 것들도 있다.

무조건 순서는 `peer` 다음 element에 `peer-*` 이어야 한다.

## <mark style="background: #BBFABBA6;">Differentiating peers</mark>

특정 peer의 상태에 따라서 스타일을 다르게 주고 싶으면 `peer/{name}`을 peer에 주고 target에는 `peer-*/{name}`(`peer-checked/{name}`)을 주면 된다.

![[Pasted image 20240423113645.png]]
```html
<fieldset>
  <legend>Published status</legend>

  <input id="draft" class="peer/draft" type="radio" name="status" checked />
  <label for="draft" class="peer-checked/draft:text-sky-500">Draft</label>
  // peer/draft가 checked 상태 일 때 자신은 text-sky-500로 바꿔라.

  <input id="published" class="peer/published" type="radio" name="status" />
  <label for="published" class="peer-checked/published:text-sky-500">Published</label>

  <div class="hidden peer-checked/draft:block">Drafts are only visible to administrators.</div>
  <div class="hidden peer-checked/published:block">Your post will be publicly visible on your site.</div>
</fieldset>
```

## <mark style="background: #BBFABBA6;">Arbitrary peers</mark>

`peer-[]` selector를 이용해서 일회용 `peer-*` 수정자를 만들 수 있다.

```html
<form>
  <label for="email">Email:</label>
  <input id="email" name="email" type="email" class="is-dirty peer" required />
  <div class="peer-[.is-dirty]:peer-required:block hidden">This field is required.</div>
  <!-- ... -->
</form>
```

```css
.peer.is-dirty:required ~ .peer-\[\.is-dirty\]\:peer-required\:block {
  display: block;
}
```

`[]` 는 attribute selecotr를 `&`은 자기자신을 나타낸다.

`_`은 공백을 나타내니 아래 코드는 이렇게 해석된다.

```html
<div>
  <input type="text" class="peer" />
  <div class="hidden peer-[:nth-of-type(3)_&]:block">
    <!-- ... -->
  </div>
</div>
```

```css
:nth-of-type(3) .peer ~ .peer-\[\:nth-of-type\(3\)_\&\]\:block {
// 세번째 자매요소아래 있는 요소만 적용
  display: block;
}
```

## <mark style="background: #FFF3A3A6;">Styling direct children</mark>

`*`을 이용해서 부모 요소에서 직계 자식 요소의 styling을 할 수 있다.

![[Pasted image 20240423114404.png]]
```html
<div>
  <h2>Categories<h2>
  <ul class="*:rounded-full *:border *:border-sky-100 *:bg-sky-50 *:px-2 *:py-0.5 dark:text-sky-300 dark:*:border-sky-500/15 dark:*:bg-sky-500/10 ...">
    <li>Sales</li>
    <li>Marketing</li>
    <li>SEO</li>
    <!-- ... -->
  </ul>
</div>
```

단 children에 직접 지정할 경우 children에 직접 지정한 것으로 override된다.

## <mark style="background: #BBFABBA6;">Styling based on descendants</mark>

`has-*`을 이용해서 자식에 기반해서 styling을 적용한다.

![[Pasted image 20240423114540.png]]
```html
<label class="has-[:checked]:bg-indigo-50 has-[:checked]:text-indigo-900 has-[:checked]:ring-indigo-200 ..">
  <svg fill="currentColor">
    <!-- ... -->
  </svg>
  Google Pay
  <input type="radio" class="checked:border-indigo-500 ..." />
</label>
```

`*`에는 pseudo-class도 있을 수 있고 (`has-[:focus]`) element selector를 넣을 수도 있다. (`has-[img]`, `has-[a]`)

## <mark style="background: #BBFABBA6;">Styling based on the descendants of a group</mark>

부모 요소의 자식을 기반으로 styling을 하려면 parent를 `group`으로 놓고 target에는 `group-has-*` 을 놓으면 된다.

![[Pasted image 20240423115152.png]]
```html
<div class="group ...">
  <img src="..." />
  <h4>Spencer Sharp</h4>
  // group인 부모가 a를 갖고 있으면 ~
  <svg class="hidden group-has-[a]:block ...">
    <!-- ... -->
  </svg>
  <p>Product Designer at <a href="...">planeteria.tech</a></p>
</div>
```

## <mark style="background: #BBFABBA6;">Styling based on the descendants of a peer</mark>

sibling element의 descendants를 기반으로 styling을 할꺼면 `peer`를 peer에 `peer-has-*`를 target에 놓으면 된다.

![[Pasted image 20240423115351.png]]
```html
<fieldset>
  <legend>Today</legend>

  <div>
    <label class="peer ...">
      <input type="checkbox" name="todo[1]" checked />
      Create a to do list
    </label>
    <svg class="peer-has-[:checked]:hidden ...">
      <!-- ... -->
    </svg>
  </div>

  <!-- ... -->
</fieldset>
```


## <mark style="background: #FFB86CA6;">Pseudo-elements</mark>

## <mark style="background: #FFF3A3A6;">Before and after</mark>

`before`와 `after`를 이용해서 `::before`, `::after`를 처리한다.

![[Pasted image 20240423115639.png]]
```html
<label class="block">
  <span class="after:content-['*'] after:ml-0.5 after:text-red-500 block text-sm font-medium text-slate-700">
    Email
  </span>
  <input type="email" name="email" class="mt-1 px-3 py-2 bg-white border shadow-sm border-slate-300 placeholder-slate-400 focus:outline-none focus:border-sky-500 focus:ring-sky-500 block w-full rounded-md sm:text-sm focus:ring-1" placeholder="you@example.com" />
</label>
```

Tailwind가 자동으로 `content:''`을 놓는다.

![[Pasted image 20240423115926.png]]
```html
<blockquote class="text-2xl font-semibold italic text-center text-slate-900">
  When you look
  <span class="before:block before:absolute before:-inset-1 before:-skew-y-3 before:bg-pink-500 relative inline-block">
    <span class="relative text-white">annoyed</span>
  </span>
  all the time, people think that you're busy.
</blockquote>
```

사실 Tailwind에서는 그냥 html 박아 넣는게 가독성 면에서 더 낫다.

```html
<blockquote class="text-2xl font-semibold italic text-center text-slate-900">
  When you look
  <span class="relative">
    <span class="block absolute -inset-1 -skew-y-3 bg-pink-500" aria-hidden="true"></span>
    <span class="relative text-white">annoyed</span>
  </span>
  all the time, people think that you're busy.
</blockquote>
```

`before`이나 `after`는 Dom에 실제로 보여지면 안되고 사용자가 선택할 수 없게 만드는 경우에만 사용하자.

`preflight base style`을 disabled하면 `content-['']` 이거 생략되지 않는다.

## <mark style="background: #FFF3A3A6;">Placeholder text</mark>

`placeholder` modifier를 이용해서 placeholder를 styling하자

![[Pasted image 20240423120354.png]]
```html
<label class="relative block">
  <span class="sr-only">Search</span>
  <span class="absolute inset-y-0 left-0 flex items-center pl-2">
    <svg class="h-5 w-5 fill-slate-300" viewBox="0 0 20 20"><!-- ... --></svg>
  </span>
  <input class="placeholder:italic placeholder:text-slate-400 block bg-white w-full border border-slate-300 rounded-md py-2 pl-9 pr-3 shadow-sm focus:outline-none focus:border-sky-500 focus:ring-sky-500 focus:ring-1 sm:text-sm" placeholder="Search for anything..." type="text" name="search"/>
</label>
```

## <mark style="background: #FFF3A3A6;">File input buttons</mark>

`file`을 이용해서 file input에 있는 button을 styling 하자.

```html
<form class="flex items-center space-x-6">
  <div class="shrink-0">
    <img class="h-16 w-16 object-cover rounded-full" src="https://images.unsplash.com/photo-1580489944761-15a19d654956?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1361&q=80" alt="Current profile photo" />
  </div>
  <label class="block">
    <span class="sr-only">Choose profile photo</span>
    <input type="file" class="block w-full text-sm text-slate-500
      file:mr-4 file:py-2 file:px-4
      file:rounded-full file:border-0
      file:text-sm file:font-semibold
      file:bg-violet-50 file:text-violet-700
      hover:file:bg-violet-100
    "/>
  </label>
</form>
```

Tailwind의 border reset은 file input button에는 적용되지 않기 때문에 border를 file input button에 추가하고 싶으면 border-style (`file:border-solid` 같은)  을 명시적으로 border-width utility와 함께 추가해야 한다.

## <mark style="background: #FFF3A3A6;">List markers</mark>

`marker` 를 이용해서 list에 있는 counter나 bullet을 styling 한다.

![[Pasted image 20240423120944.png]]
```html
<ul role="list" class="marker:text-sky-400 list-disc pl-5 space-y-3 text-slate-500">
  <li>5 cups chopped Porcini mushrooms</li>
  <li>1/2 cup of olive oil</li>
  <li>3lb of celery</li>
</ul>
```

`marker`는 상속되게 만들어져 있다. 그래서 li에 직접 디자인 하지 않으면 부모요소에 있는 게 내려온다.

## <mark style="background: #FFF3A3A6;">Highlighted text</mark>

`selection` 를 이용해서 active text selection을 styling한다.

![[Pasted image 20240423122009.png]]
```html
<div class="selection:bg-fuchsia-300 selection:text-fuchsia-900">
  <p>
    So I started to walk into the water. I won't lie to you boys, I was
    terrified. But I pressed on, and as I made my way past the breakers
    a strange calm came over me. I don't know if it was divine intervention
    or the kinship of all living things but I tell you Jerry at that moment,
    I <em>was</em> a marine biologist.
  </p>
</div>
```

`selection`도 `marker`와 같게 상속되게 만들어져있다. `<body>`에 다가 "selection:bg-pink-300" 해놓으면 모든 active text selection들이 pink 색이 된다.

## <mark style="background: #FFF3A3A6;">First-line and first-letter</mark>

![[Pasted image 20240423122428.png]]
```html
<p class="first-line:uppercase first-line:tracking-widest
  first-letter:text-7xl first-letter:font-bold first-letter:text-slate-900
  first-letter:mr-3 first-letter:float-left
">
  Well, let me tell you something, funny boy. Y'know that little stamp, the one
  that says "New York Public Library"? Well that may not mean anything to you,
  but that means a lot to me. One whole hell of a lot.
</p>
```

## <mark style="background: #FFF3A3A6;">Dialog backdrops</mark>

`<dialog>` 의 backdrop을 styling하고 싶으면 `backdrop` 을 사용하면 된다.
```html
<dialog class="backdrop:bg-gray-50">
  <form method="dialog">
    <!-- ... -->
  </form>
</dialog>
```

`open` 이라는 것도 dialog와 관련되어 있다.

## <mark style="background: #FFB86CA6;">Media and feature queries</mark>

## <mark style="background: #FFF3A3A6;">Responsive breakpoints</mark>

특정 breakpoint를 styling하고 싶으면 responsive modifier를 사용한다. `md`, `lg`

```html
<div class="grid grid-cols-3 md:grid-cols-4 lg:grid-cols-6"> 
	<!-- ... --> 
</div>
```

`3-column grid`를 mobile에 적용하고 4를 medium 그리고 6을 large-width에 넣는 로직이다.

## <mark style="background: #FFF3A3A6;">Prefers color scheme</mark>

`prefers-color-scheme` media query는 user가 light theme을 좋아하는지 dark theme을 좋아하는지 알려준다.

이 설정들은 os level에서 설정된다.

`dark` modifier로 dark mode를 지원하고 그냥 적어서 light mode를 사용할 수 있다.

![[Pasted image 20240423144037.png]]

```html
<div class="bg-white dark:bg-slate-900 rounded-lg px-6 py-8 ring-1 ring-slate-900/5 shadow-xl">
  <div>
    <span class="inline-flex items-center justify-center p-2 bg-indigo-500 rounded-md shadow-lg">
      <svg class="h-6 w-6 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true"><!-- ... --></svg>
    </span>
  </div>
  <h3 class="text-slate-900 dark:text-white mt-5 text-base font-medium tracking-tight">Writes Upside-Down</h3>
  <p class="text-slate-500 dark:text-slate-400 mt-2 text-sm">
    The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
  </p>
</div>
```

## <mark style="background: #FFF3A3A6;">Prefers reduced motion</mark>

`prefers-reduced-motion` media query는 유저가 non-essential motion을 최소화했는지 알려준다.

`motion-reduce` 를 이용하면 된다.

dev-tool에서 ctrl+shift+p > reduced 해보면 적용된 모습을 볼 수 있다. (적용하면 Processing 앞에 저 동그라미 사라짐)

![[Pasted image 20240423144419.png]]
```html
<button type="button" class="bg-indigo-500 ..." disabled>
  <svg class="motion-reduce:hidden animate-spin ..." viewBox="0 0 24 24"><!-- ... --></svg>
  Processing...
</button>
```


non-essential motion을 적용하지 않은 상태만을 찍으려면 `motion-safe` 를 적용하면 된다.

## <mark style="background: #FFF3A3A6;">Prefers contrast</mark>

`prefers-contrast` media query는 유저가 contrast를 더 혹은 덜 request했는지를 보여준다.

`contrast-more` 은 more contrast를 요청하면 styling을 한다.

`contrast-less` 는 그 반대이다.

![[Pasted image 20240423145830.png]]
```html
<form>
  <label class="block">
    <span class="block text-sm font-medium text-slate-700">Social Security Number</span>
    <input class="border-slate-200 placeholder-slate-400 contrast-more:border-slate-400 contrast-more:placeholder-slate-500"/>
    <p class="mt-2 opacity-10 contrast-more:opacity-100 text-slate-600 text-sm">
      We need this to steal your identity.
    </p>
  </label>
</form>
```

## <mark style="background: #FFF3A3A6;">Forced colors mode</mark>

`forced-colors` media query는 forced colors mode를 유저가 쓰는지 확인한다.

기존 site의 색을 override한다. 

devtool > ctrl+shift+p > forced color active

![[Pasted image 20240423150354.png]]
```html
<form>
  <legend> Choose a theme: </legend>
  <label>
    <input type="radio" class="forced-colors:appearance-auto appearance-none" />
    <p class="forced-colors:block hidden">
      Cyan
    </p>
    <div class="forced-colors:hidden h-6 w-6 rounded-full bg-cyan-200 ..."></div>
    <div class="forced-colors:hidden h-6 w-6 rounded-full bg-cyan-500 ..."></div>
  </label>
  <!-- ... -->
</form>
```

## <mark style="background: #FFF3A3A6;">Viewport orientation</mark>

`portrait`(세로)와 `landscape`(가로)을 이용해서 viewport에 따라서 스타일을 변경할 수 있다.

휴대폰을 가로로 놓냐 세로로 놓냐 이거임.

```html
<div>
  <div class="portrait:hidden">
    <!-- ... -->
  </div>
  <div class="landscape:hidden">
    <p>
      This experience is designed to be viewed in landscape. Please rotate your
      device to view the site.
    </p>
  </div>
</div>
```

## <mark style="background: #FFF3A3A6;">Print styles</mark>

`print`을 이용해서 document가 printed될 때 스타일을 적용한다.

```html
<div>
  <article class="print:hidden">
    <h1>My Secret Pizza Recipe</h1>
    <p>This recipe is a secret, and must not be shared with anyone</p>
    <!-- ... -->
  </article>
  <div class="hidden print:block">
    Are you seriously trying to print this? It's secret!
  </div>
</div>
```

## <mark style="background: #FFF3A3A6;">Supports rules</mark>

`supports-[...]`를 이용해서 유저의 브라우저가 해당 feature를 지원하냐에 따라서 스타일을 줄 수 있다.

```html
<div class="flex supports-[display:grid]:grid ..."> 
	<!-- ... --> 
</div>
```

내부적으로 `@supports rules` 를 만들어내며 `[]`안에 적어내는 모든 걸 `@supports (...)` 안에 넣는다. 
( `and` , `or` , property/value pair 모든 것이 다 포함된다.)

property의 경우 property 이름만 적어도 된다. (`supports-[backdrop-filter]:`)

`tailwind.config.js`에서 `theme.supports`에 shortcut을 만들 수 있다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    supports: {
      grid: 'display: grid',
    },
  },
}
```

설정에 grid를 넣었기 때문에 이후 supports-grid 처럼 쓸 수 있다.

## <mark style="background: #FFB86CA6;">Attribute selectors</mark>

## <mark style="background: #FFF3A3A6;">ARIA states</mark>

`ARIA`: disabilites을 위해서 웹 접근성을 높히는 법칙과 state들, html에 이미 존재해서 이것을 sementic하게 만든다면 필요없다.

`aria-*` 을 이용해서 `ARIA attributes` 을 styling할 수 있다.

```html
<div aria-checked="true" class="bg-gray-600 aria-checked:bg-sky-700">
  <!-- ... -->
</div>
```

| Modifier        | CSS                       |
| --------------- | ------------------------- |
| `aria-busy`     | `&[aria-busy=“true”]`     |
| `aria-checked`  | `&[aria-checked=“true”]`  |
| `aria-disabled` | `&[aria-disabled=“true”]` |
| `aria-expanded` | `&[aria-expanded=“true”]` |
| `aria-hidden`   | `&[aria-hidden=“true”]`   |
| `aria-pressed`  | `&[aria-pressed=“true”]`  |
| `aria-readonly` | `&[aria-readonly=“true”]` |
| `aria-required` | `&[aria-required=“true”]` |
| `aria-selected` | `&[aria-selected=“true”]` |

`tailwind.config.js` 에서 사용가능한 aria를 설정할 수 있다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    extend: {
      aria: {
        asc: 'sort="ascending"',
        desc: 'sort="descending"',
      },
    },
  },
};
```



한번만 쓰거나 너무 복잡하면 `[]`을 이용해서 임시적으로 나타낼 수 있다.

![[Pasted image 20240423152047.png]]
```html
<table>
  <thead>
    <tr>
      <th
        aria-sort="ascending"
        class="aria-[sort=ascending]:bg-[url('/img/down-arrow.svg')] aria-[sort=descending]:bg-[url('/img/up-arrow.svg')]"
      >
        Invoice #
      </th>
      <!-- ... -->
    </tr>
  </thead>
  <!-- ... -->
</table>
```

aria도 sibling, group 전부 가능하다.

```html
<table>
  <thead>
    <tr>
    <th aria-sort="ascending" class="group">
      Invoice #
      <svg class="group-aria-[sort=ascending]:rotate-0 group-aria-[sort=descending]:rotate-180"><!-- ... --></svg>
    </th>
    <!-- ... -->
    </tr>
  </thead>
  <!-- ... -->
</table>
```

## <mark style="background: #FFF3A3A6;">Data attributes</mark>

`data-*` 을 이용해서 data attribute를 기반으로 styling을 한다.

```html
<!-- Will apply -->
<div data-size="large" class="data-[size=large]:p-8">
  <!-- ... -->
</div>

<!-- Will not apply -->
<div data-size="medium" class="data-[size=large]:p-8">
  <!-- ... -->
</div>
```

`tailwind.config.js` 에서 `theme.data` 를 바꿔서 data attribute에 대한 shortcut을 설정할 수 있다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    data: {
      checked: 'ui~="checked"',
    },
  },
}
```

```html
<div data-ui="checked active" class="data-checked:underline"> 
	<!-- ... --> 
</div>
```

## <mark style="background: #FFF3A3A6;">RTL support</mark>

`rtl`, `ltr` 을 이용해서 writing mode에 대한 styling을 할 수 있다.

![[Pasted image 20240423153147.png]]
```html
<div class="group flex items-center">
  <img class="shrink-0 h-12 w-12 rounded-full" src="..." alt="" />
  <div class="ltr:ml-3 rtl:mr-3">
    <p class="text-sm font-medium text-slate-700 group-hover:text-slate-900">...</p>
    <p class="text-sm font-medium text-slate-500 group-hover:text-slate-700">...</p>
  </div>
</div>
```

```html
<html dir="ltr">
	// 이 속성을 이용해서 조정한다.
</html>
```

rtl, ltr같은 것이 안되는 single direction page일때는 적용 안된다.

## <mark style="background: #FFF3A3A6;">Open/closed state</mark>

`<details>` 나 `<dialog>` 같은 element가 열린 상태일 때 styling을 입히기 위해서 `open`을 사용한다.
![[Pasted image 20240423153543.png]]
![[Pasted image 20240423153535.png]]
```html
<div class="max-w-lg mx-auto p-8">
  <details class="open:bg-white dark:open:bg-slate-900 open:ring-1 open:ring-black/5 dark:open:ring-white/10 open:shadow-lg p-6 rounded-lg" open>
    <summary class="text-sm leading-6 text-slate-900 dark:text-white font-semibold select-none">
      Why do they call it Ovaltine?
    </summary>
    <div class="mt-3 text-sm leading-6 text-slate-600 dark:text-slate-400">
      <p>The mug is round. The jar is round. They should call it Roundtine.</p>
    </div>
  </details>
</div>
```

## <mark style="background: #FF5582A6;">Custom modifiers</mark>

## <mark style="background: #FFB86CA6;">Using arbitrary variants</mark>

`[]`을 이용해서 임의로 custom해서 utility class를 만들 수 있다.

`&`은 자기 자신을 의미한다.

```html
<ul role="list">
  {#each items as item}
    <li class="[&:nth-child(3)]:underline">{item}</li>
  {/each}
</ul>
```

`[]` 안에 `_`를 넣어서 공백을 줄 수 있으며 `@media`, `@supports` 등 여러가지 것들을 `[]`안에 담아서 보현할 수 있다.(at rule custom modifier의 경우 `&`가 필요 없다.)

`at-rule` 뒤에 curly braces(`{}`) 을 넣고 selector를 담을 수도 있다. (그냥 CSS처럼)

```html
<button type="button" class="[@media(any-hover:hover){&:hover}]:opacity-100">
  <!-- ... -->
</button>
```

## <mark style="background: #FFB86CA6;">Creating a plugin</mark>

`addVariant` API를 이용해서 자주 사용하는 modifier를 plugin으로 추출할 수 있다.

```js
let plugin = require('tailwindcss/plugin')

module.exports = {
  // ...
  plugins: [
    plugin(function ({ addVariant }) {
      // Add a `third` variant, ie. `third:pb-0`을 적어도 '&:nth-child(3)'이 된다.
      addVariant('third', '&:nth-child(3)')
    })
  ]
}
```

## <mark style="background: #FFB86CA6;">Advanced topics</mark>

## <mark style="background: #FFF3A3A6;">Using with your own classes</mark>

tailwind's layer나 plugin에 저장했다면 custom class를 만들 수 있다.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer utilities {
  .content-auto {
    content-visibility: auto;
  }
}
```

```html
<div class="lg:content-auto">
  <!-- ... -->
</div>
```

## <mark style="background: #FFF3A3A6;">Ordering stacked modifiers</mark>

modifier 호출의 경우 다음과 같은 과정을 거친다.

```
'dark:group-hover:focus:opacity-100' 

// 내부적으론 이렇다.
dark(groupHover(focus('opacity-100')))
```

순서 때문에 의미가 바뀌는 경우가 존재한다.

```css
/* dark:group-hover:opacity-100 */ 
.dark .group:hover .dark\:group-hover\:opacity-100 {
	opacity: 1; 
} 
/* group-hover:dark:opacity-100 */ 
.group:hover .dark .group-hover\:dark\:opacity-100 {
	opacity: 1; 
}
```

`prose-headings` 에 경우에도 그렇다.

```css
/* prose-headings:hover:underline */
.prose-headings\:hover\:underline:hover :is(:where(h1, h2, h3, h4, th)) {
  text-decoration: underline;
}

/* hover:prose-headings:underline */
.hover\:prose-headings\:underline :is(:where(h1, h2, h3, h4, th)):hover {
  text-decoration: underline;
}
```

첫번째 줄은 모든 첫 heading이 article을 hover했을 때 underlined되는 것인데

두번째 줄은 hover했을 때만 각각 heading이 underlined된다.

## <mark style="background: #FFF3A3A6;">Appendix</mark>

https://tailwindcss.com/docs/hover-focus-and-other-states#quick-reference

## <mark style="background: #FF5582A6;">Dark Mode</mark>

`dark` variant를 이용해서 dark mode일때 styling을 할 수 있다.

![[Pasted image 20240423161318.png]]
```html
<div class="bg-white dark:bg-slate-800 rounded-lg px-6 py-8 ring-1 ring-slate-900/5 shadow-xl">
  <div>
    <span class="inline-flex items-center justify-center p-2 bg-indigo-500 rounded-md shadow-lg">
      <svg class="h-6 w-6 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true"><!-- ... --></svg>
    </span>
  </div>
  <h3 class="text-slate-900 dark:text-white mt-5 text-base font-medium tracking-tight">Writes Upside-Down</h3>
  <p class="text-slate-500 dark:text-slate-400 mt-2 text-sm">
    The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
  </p>
</div>
```

기본적으로 `prefers-color-scheme` media query를 쓰지만 `selector` strategy를 이용해서 dark mode를 toggle할 수 있다.

## <mark style="background: #FFB86CA6;">Toggling dark mode manually</mark>

`selector` strategy를 `media` 대신에 사용해서 dark mode를 toggle할 수 있다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: 'selector',
  // ...
}
```

이제는 html의 class에 따라서 dark mode의 여부가 결정된다.
```html
<!-- Dark mode not enabled -->
<html>
<body>
  <!-- Will be white -->
  <div class="bg-white dark:bg-black">
    <!-- ... -->
  </div>
</body>
</html>

<!-- Dark mode enabled -->
<html class="dark">
<body>
  <!-- Will be black -->
  <div class="bg-white dark:bg-black">
    <!-- ... -->
  </div>
</body>
</html>
```

`dark` class에 적용할 때 prefix를 tailwind config에 지정했다면 `dark`앞에 prefix를 붙혀서 적용시킨다.
(`tw-dark`)

## <mark style="background: #FFB86CA6;">Customizing the selector</mark>

어떤 frameworks은 `dark` mode가 active일 때 다른 class name을 붙히거나 다크 모드를 더하는 접근법을 갖고 있다.

`darkMode` 를 array로 바꿔서 config에 집어 넣으면 custom할 수 있다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ['selector', '[data-mode="dark"]'],
  // ...
}
```

:where을 붙혀 버린다.

```css
.dark\:underline:where([data-mode="dark"], [data-mode="dark"] *){ 
	text-decoration-line: underline 
}
```

## <mark style="background: #FFB86CA6;">Supporting system preference and manual selection</mark>

`selection` strategy는 `window.matchMedia()` API를 활용해서 유저 기본 설정과 manual selection을 지원한다.

다음은 OS 기본 설정을 유지하면서 light mode, dark mode를 지원하는 예시이다.

```js
// On page load or when changing themes, best to add inline in `head` to avoid FOUC
if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
  document.documentElement.classList.add('dark')
} else {
  document.documentElement.classList.remove('dark')
}

// Whenever the user explicitly chooses light mode
localStorage.theme = 'light'

// Whenever the user explicitly chooses dark mode
localStorage.theme = 'dark'

// Whenever the user explicitly chooses to respect the OS preference
localStorage.removeItem('theme')
```

OS 기본 설정과 manual selection 중 어떤 걸 기반으로 할지 설정할 수 있는 것이다.

## <mark style="background: #FFB86CA6;">Overriding the dark variant</mark>

custom variant로 dark variant를 overriding하고 싶으면 `variant` darkMode strategy를 사용하면 된다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ['variant', '&:not(.light *)'],
  // ...
}
```

이제 해당 selector는 Tailwind가 건들이지 않기에 specificity를 잘 맞춰서 작업해주면 된다.

## <mark style="background: #FFF3A3A6;">Using multiple selectors</mark>

만약 dark mode가 허용되야 하는 다양한 상황들이 있다면 배열을 제공해서 다 담으면 된다.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ['variant', [
    '@media (prefers-color-scheme: dark) { &:not(.light *) }',
    '&:is(.dark *)',
  ]],
  // ...
}
```

## <mark style="background: #FF5582A6;">Reusing Styles</mark>

## <mark style="background: #FFB86CA6;">Using editor and language features</mark>

한 파일에서 반복이 될때는 multi-cursor랑 loops가 제일 쉽다.

## <mark style="background: #FFB86CA6;">Multi-cursor editing</mark>

alt 클릭 혹은 ctrl alt 방향키 로 mutli-cursor

## <mark style="background: #FFB86CA6;">Loops</mark>

한 component나 custom class를 추출하기 전에 한번 이상 template에서 쓰는지 확인해라.

## <mark style="background: #FFB86CA6;">Extracting classes with @apply</mark>

`@apply` 를 사용해서 반복되는 utility patterns을 custom CSS class로 추출할 수 있다.

```html
<!-- Before extracting a custom class --> <button class="py-2 px-5 bg-violet-500 text-white font-semibold rounded-full shadow-md hover:bg-violet-700 focus:outline-none focus:ring focus:ring-violet-400 focus:ring-opacity-75"> 
	Save changes 
</button> 

<!-- After extracting a custom class --> 
<button class="btn-primary"> 
	Save changes 
</button>
```

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
	.btn-primary {
		 @apply py-2 px-5 bg-violet-500 text-white font-semibold rounded-full shadow-md hover:bg-violet-700 focus:outline-none focus:ring focus:ring-violet-400 focus:ring-opacity-75; 
	 } 
 }
```

## <mark style="background: #FFB86CA6;">Avoiding premature abstraction</mark>

@apply 난사해서 돌아가지 마라.

## <mark style="background: #FF5582A6;">Adding Custom Styles</mark>

## <mark style="background: #FFB86CA6;">Customizing your theme</mark>

`tailwind.config.js` 에서 `theme`  section을 수정해서 color palette, spacing scale, typography scale, or breakpoints를 설정해라.

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    screens: {
      sm: '480px',
      md: '768px',
      lg: '976px',
      xl: '1440px',
    },
    colors: {
      'blue': '#1fb6ff',
      'pink': '#ff49db',
      'orange': '#ff7849',
      'green': '#13ce66',
      'gray-dark': '#273444',
      'gray': '#8492a6',
      'gray-light': '#d3dce6',
    },
    fontFamily: {
      sans: ['Graphik', 'sans-serif'],
      serif: ['Merriweather', 'serif'],
    },
    extend: {
      spacing: {
        '128': '32rem',
        '144': '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      }
    }
  }
}
```

## <mark style="background: #FFB86CA6;">Using arbitrary values</mark>

`[]` 안에 arbitrary values를 집어 넣고 클래스를 박아넣으면 적용된다.

```html
<div class="top-[117px]"> 
	<!-- ... --> 
</div>
```

hover나 mediaquery같은것과 같이 쓸수도 있다.

```html
<div class="top-[117px] lg:top-[344px]">
  <!-- ... -->
</div>
```

이것들은 bg color, font sizes, pseudo-element content 다 적용된다.

```html
<div class="bg-[#bada55] text-[22px] before:content-['Festivus']">
  <!-- ... -->
</div>
```

이건 `theme` function에서도 사용가능하다.

```html
<div class="grid grid-cols-[fit-content(theme(spacing.32))]"> 
	<!-- ... --> 
</div>
```

CSS variable쓸꺼면 그냥 변수명 적으면 된다.

```html
<div class="bg-[--my-color]"> 
	<!-- ... --> 
</div>
```

## <mark style="background: #FFB86CA6;">Arbitrary  properties</mark>

Tailwind에서 제공하지 않는 CSS property가 있을 때는 `[]`을 이용해서 해결한다.

```html
<div class="[mask-type:luminance]"> 
	<!-- ... --> 
</div>
```

다른 modifier와 함께 쓸 수 있다. (`hover:[]`)

CSS Variable을 사용할 때 같이 쓰면 좋다.

```html
<div class="[--scroll-offset:56px] lg:[--scroll-offset:44px]"> 
	<!-- ... --> 
</div>
```

## <mark style="background: #FFB86CA6;">Arbitrary variants</mark>

Arbitrary variants는 arbitrary value와 비슷하지만 즉석 선택자 수정에 필요합니다. `hover:`나 `md:` 뒤에 바로 `[]`와 함께 나옴

```html
<ul role="list">
  {#each items as item}
    <li class="lg:[&:nth-child(3)]:hover:underline">{item}</li>
  {/each}
</ul>
```

## <mark style="background: #FFF3A3A6;">Handling whitespace</mark>

`_`을 이용해서 공백을 처리한다.
(`grid-cols-[1fr_500px_1fr]`)

`_`을 써야하는 경우에는 backslash (`/`)을 사용한다.

JSX의 경우 backslash가 HTML에서 제거되기 때문에 String.raw\`class이름\` 을 backslash를 처리하지 않게해야한다.

```jsx
<div className={String.raw`before:content-['hello\_world']`}> 
	<!-- ... --> 
</div>
```

## <mark style="background: #FFF3A3A6;">Resolving ambiguities</mark>

`text-black` 과 `text-lg` 는 앞에 `text`는 같지만 카테고리가 다르다.

헷갈리는 이걸 arbitrary value로 해결할 수 있다.

```html
<!-- Will generate a font-size utility --> 
<div class="text-[22px]">
	...
</div>

<!-- Will generate a color utility --> 
<div class="text-[#bada55]">
	...
</div>
```

variable을 사용해도 헷갈리게 될 수 있는데

```html
<div class="text-[var(--my-var)]">
	...
</div>
```

[[https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Types|CSS data type]]을 앞에 적어서 Tailwind에 힌트를 줄 수 있다.

```html
<!-- Will generate a font-size utility -->
<div class="text-[length:var(--my-var)]">...</div>

<!-- Will generate a color utility -->
<div class="text-[color:var(--my-var)]">...</div>
```

## <mark style="background: #FFB86CA6;">Using CSS and @layer</mark>

`@layer` directive를 이용해서 Tailwind의 base, components, utilities layer에 style을 더할 수 있다.

```css
@tailwind base; 
@tailwind components; 
@tailwind utilities; 

@layer components {
	.my-custom-style {
	 /* ... */ } 
 }
```

Tailwind는 같은 specifitity 일때 순서로 결정되는 CSS의 문제를 layer로 해결했는데 각각 다음과 같다.

| layer      | 설명                                                |
| ---------- | ------------------------------------------------- |
| base       | reset rules, HTML element의 default styles         |
| components | class-based styles인데 utilities에 override될 수 있는 애들 |
| utilities  | 우선순위가 가장 강한 작고 하나의 목적을 가진 클래스들                    |

그래서 그냥 custom css 박는 것보다 `@layer`를 이용하면 기존의 틀에서 크게 벗어나지는 않으면서 스타일은 적용할 수 있게 된다.
(`@tailwind` directive에 있는 style이 재 적립되며 modifier나 tree-shaking 다 사용 가능하게 된다.)

## <mark style="background: #FFF3A3A6;">Adding Base Styles</mark>

특정 style을 전체 적용 할꺼면 `html`, `body`에다가 class추가하면 되는데 만약 너만의 기본 base style을 만들고 싶으면 `@layer` 로 base를 찍어서 올리면 된다.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  h1 {
    @apply text-2xl;
  }
  h2 {
    @apply text-xl;
  }
  /* ... */
}
```

theme에 정의된 value를 참조해서 custom base style을 만들려면 `theme` function이나 `@apply` directive를 쓰면 된다.

## <mark style="background: #FFF3A3A6;">Adding component classes</mark>

`components` layer를 이용해서 utility class에 override는 되도 되지만 바꾸고 싶은 복잡한 클래스를 더할 수 있다.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .card {
    background-color: theme('colors.white');
    border-radius: theme('borderRadius.lg');
    padding: theme('spacing.6');
    box-shadow: theme('boxShadow.xl');
  }
  /* ... */
}
```

해당 클래스(`.card`)를 class에 집어넣어서 사용할 수 있다. 단, utility class에 잡아먹힐 수 있다. (ex `rounded-none`)

third-party components에 custom style을 넣을 때도 사용한다.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .select2-dropdown {
    @apply rounded-b-lg shadow-md;
  }
  .select2-search {
    @apply border border-gray-300 rounded;
  }
  .select2-results__group {
    @apply text-lg font-bold text-gray-900;
  }
  /* ... */
}
```

theme에 정의된 value를 참조해서 custom component style을 만들려면 `theme` function이나 `@apply` directive를 쓰면 된다.

## <mark style="background: #FFF3A3A6;">Adding custom utilities</mark>

`utilities` layer를 이용해서 custom utility class를 적용한다.

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer utilities {
  .content-auto {
    content-visibility: auto;
  }
}
```

tailwind에 없는 style을 적용할 때 좋다.

## <mark style="background: #FFF3A3A6;">Using modifiers with custom CSS</mark>

위의 예시들처럼 `@layer`로 layer에 담은 style은 modifier 지원을 받는다 (ex hover states, responsive breakpoints, dark mode ... )

```html
<div class="lg:dark:content-auto"> 
	<!-- ... --> 
</div>
```

## <mark style="background: #FFF3A3A6;">Removing unused custom CSS</mark>

`@layer`를 사용해서 담은 style은 unused상태이면 compile되지 않고 삭제된다. `@layer` 없이 그냥 css를 적으면 그런 과정이 없어지고 영원히 남아있음.

이때 `@tailwind` layer들과 순서를 맞춰서 놓음으로써 utilities가 여전히 override할 수 있게 할 수 있다.

```css
@tailwind base; 
@tailwind components; 
/* This will always be included in your compiled CSS */ 
.card {
	/* ... */ 
} 
@tailwind utilities;
```

## <mark style="background: #FFB86CA6;">Using multiple CSS files</mark>

css작업 끝에 항상 하나의 CSS파일로 모이게 해야한다.

그렇지 않으면 `@layer`를 `@tailwind` 없이 사용해서 오류가 발생하게 됨.

`postcss-import` plugin으로 쉽게 바꿀 수 있다.

[[https://tailwindcss.com/docs/using-with-preprocessors#build-time-imports|build-time imports]]

```js
// postcss.config.js
module.exports = {
  plugins: {
    'postcss-import': {},
    tailwindcss: {},
    autoprefixer: {},
  }
}
```

## <mark style="background: #FFB86CA6;">Layers and per-component CSS</mark>

vue나 svelte 같은 library는 component단의 style block을 지원하는데 이걸 쓰게 되면 각 style block을 Tailwind가 독

립된 요소로 생각해서 `@layer`가 적용되지 않게 된다. 그냥 css쓰면 되긴 하는데 그러면 Tailwind 왜 씀

따라서 그냥 Tailwind를 쓰는게 좋다.

## <mark style="background: #FFB86CA6;">Writing plugins</mark>

CSS file대신에 Tailwind.config.js에서 Tailwind plugin system을 사용해서 custom style을 입힐 수 있다.

```js
// tailwind.config.js
const plugin = require('tailwindcss/plugin')

module.exports = {
  // ...
  plugins: [
    plugin(function ({ addBase, addComponents, addUtilities, theme }) {
      addBase({
        'h1': {
          fontSize: theme('fontSize.2xl'),
        },
        'h2': {
          fontSize: theme('fontSize.xl'),
        },
      })
      addComponents({
        '.card': {
          backgroundColor: theme('colors.white'),
          borderRadius: theme('borderRadius.lg'),
          padding: theme('spacing.6'),
          boxShadow: theme('boxShadow.xl'),
        }
      })
      addUtilities({
        '.content-auto': {
          contentVisibility: 'auto',
        }
      })
    })
  ]
}
```

[[https://tailwindcss.com/docs/plugins|Plugins]]
