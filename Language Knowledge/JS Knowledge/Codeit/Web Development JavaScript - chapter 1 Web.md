## <mark style="background: #FF5582A6;">웹 기초</mark>

## <mark style="background: #FFB86CA6;">URL(Uniform Resource Location)</mark>

![[Pasted image 20240329110608.png]]

## <mark style="background: #FFF3A3A6;">Protocol</mark>

프로토콜의 이름을 Scheme이라고 한다.

프로토콜의 종류 : https (HyperText Transfer Protocol + Secure), TCP, IP, ethernet

## <mark style="background: #FFF3A3A6;">Host</mark>

서버를 특정하는 주소

hostname(Domain Name) 은 Domain Name Resolution을 거치는데 이 과정을 간단하게 해보면

> Web browser > 
> Name Server > 
> 다른 Name Server >
> 해당 ip 주소 찾음 >
> Web browser 와 연결되고 Web browser는 캐시에 저장하고 다음부턴 캐시에 있는걸 꺼내 쓴다.

## <mark style="background: #FFF3A3A6;">Path</mark>

서버에 있는 데이터 중 원하는 데이터를 특정

## <mark style="background: #FFF3A3A6;">query</mark>

데이터에 관한 세부적인 요구사항

`queryname = queryvalue`

## <mark style="background: #FFB86CA6;">request</mark>

서버로 보내는 요청

## <mark style="background: #FFF3A3A6;">Headers</mark>

Request에 관련된 key와 value 쌍으로 구성된 정보들 (개발자 도구 Network에서 확인)

## <mark style="background: #FFF3A3A6;">Request 종류</mark>

## <mark style="background: #BBFABBA6;">HTTP Method</mark>

## <mark style="background: #ABF7F7A6;">GET</mark>

기존 데이터를 조회

## <mark style="background: #ABF7F7A6;">POST</mark>

새 데이터 추가

Request Body가 필요(뭘로 할건지 알아야되니깐)

## <mark style="background: #ABF7F7A6;">PUT</mark>

기존 데이터 수정,  덮어쓰기

Request Body가 필요(뭘로 할건지 알아야되니깐)

## <mark style="background: #ABF7F7A6;">DELETE</mark>

기존 데이터 삭제

## <mark style="background: #ABF7F7A6;">PATCH</mark>

기존의 데이터를 수정할 때 사용하는 메소드, 새 데이터로 기존 데이터의 일부를 수정하려고 할 때 쓰는 메소드

## <mark style="background: #ABF7F7A6;">HEAD</mark>

GET과 비슷하지만 response에서 head부분만 받는다.

서버로부터 용량이 큰 파일을 받을 때 용량의 크기만 조사(Content-length header로 용량을 얻어옴) 같은 상황에서 쓰인다.

## <mark style="background: #FFF3A3A6;">Request시 URL</mark>

특정 id가 필요한 작업 (직원의 id)을 수행할 경우 URL에 추가할 내용이 존재한다.

```
	https://DomainName/path/특정한 id
```

직원 정보를 특정할 수 있도록 URL 끝에 고유 식별자를 붙여야 한다.

## <mark style="background: #FFF3A3A6;">Request 과정</mark>

1. Request를 보내면 Domain Name을 보고 Domain Name Resolution을 통해 ip주소를 받는다.
2. 서버로 URL에서 path 이후의 부분들을 Request에 담아서 보낸다.
3. 서버에서는 데이터를 받고 처리한 후 그걸 다시 Response에 담아서 보낸다.
4. Response 내용에 따라서 웹 브라우저가 작동된다.

## <mark style="background: #FFF3A3A6;">한번 접속할 때 Request가 한번 필요할까?</mark>

<b>아니다. </b>

예를 들어 html을 response로 받았다고 하자. 
html 파일 안에 있는 이미지가 src가 있다고 하면 웹 브라우저는 다시 request를 보내서 이미지를 받아와야 한다. 
따라서 보통 이런 것들 때문에 한번 이상의 request-response 통신이 오고 간다고 생각하는게 좋다.

![[Pasted image 20240329113548.png]]
그냥 구글 홈페이지 인데 <b>63</b>개의 request가 필요했다. (extension에 따라서 다름)

## <mark style="background: #FFB86CA6;">response</mark>

서버가 보내는 요청

## <mark style="background: #FFF3A3A6;">구조</mark>

## <mark style="background: #BBFABBA6;">Head</mark>

## <mark style="background: #ABF7F7A6;">Status Code</mark>

```javascript
fetch('url')
	.then((response)=>{
		response.status;
	});
```

request에 대한 결과를 나타내는 코드

100 ~ 500까지 여러가지가 있으며 각각 대응되는 상태 메세지가 있다.

| 번대                                             | Status Code && 이름         | 설명                                                                                                                                                                                                |
| ---------------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 100<br>(Informational Response)                | 100 Continue              | 클라이언트가 서버에게 계속 리퀘스트를 보내도 괜찮은지 물어봤을 때, 리퀘스트를 보내도 괜찮다고 알려주는 상태 코드                                                                                                                                   |
|                                                | 101 Switching Protocols   | 클라이언트가 프로토콜을 바꾸자는 리퀘스트를 보냈을 때, 그 프로토콜로 전환한다는 코드                                                                                                                                                   |
| 200<br>(리퀘스트가 성공 처리)                           | 200 OK                    | 리퀘스트가 성공적으로 처리되었음                                                                                                                                                                                 |
|                                                | 201 Created               | 리퀘스트의 내용대로 리소스가 잘 생성되었음.                                                                                                                                                                          |
|                                                | 202 Accepted              | 리퀘스트가 접수되었음.                                                                                                                                                                                      |
| 300<br>(리퀘스트가 아직 처리되지 않았고, 추가 작업이 필요함.)        | 301 Moved Permanently     | 리소스의 위치가 바뀌었음. 이때 Location이라는 헤더도 함께 포함, (새 URL이 담겨 있음, 브라우저가 자동으로 여기에 리다이렉션 보냄)                                                                                                                  |
|                                                | 302 Found                 | 리소스의 위치가 일시적으로 바뀌었음.(새 URL이 담겨 있음, 브라우저가 자동으로 여기에 리다이렉션 보냄)                                                                                                                                       |
|                                                | 304 Not Modified          | 브라우저에서 response로 한번 받아서 저장해놓은 리소스들(캐시)과 서버에서 보낼 리소스가 같을 경우 304와 함께 리소스를 안 보냄                                                                                                                      |
| 400<br>(리퀘스트를 보내는 클라이언트 쪽에 문제가 있음을 의미하는 상태 코드) | 400 Bad Request           | 말그대로 리퀘스트에 문제가 있음을 나타냅니다. 리퀘스트 내부 내용의 문법에 오류가 존재하는 등의 이유로 인해 발생합니다.                                                                                                                               |
|                                                | 401 Unauthorized          | 아직 신원이 확인되지 않은(unauthenticated) 사용자로부터 온 리퀘스트를 처리할 수 없다는 뜻입니다.                                                                                                                                    |
|                                                | 403 Forbidden             | 사용자의 신원은 확인되었지만 해당 리소스에 대한 접근 권한이 없는 사용자라서 리퀘스트를 처리할 수 없다는 뜻입니다.                                                                                                                                  |
|                                                | 404 Not Found             | 해당 URL이 나타내는 리소스를 찾을 수 없다는 뜻입니다. 보통 이런 상태 코드가 담긴 리스폰스는 그 바디에 관련 웹 페이지를 이루는 코드를 포함하고 있는 경우가 많습니다.                                                                                                  |
|                                                | 405 Method Not Allowed    | 해당 리소스에 대해서 요구한 처리는 허용되지 않는다는 뜻입니다. 만약 어떤 서버의 이미지 파일을 누구나 조회할 수는 있지만 아무나 삭제할 수는 없다고 해봅시다. GET 리퀘스트는 허용되지만, DELETE 메소드는 허용되지 않는 상황인 건데요. 그런데 만약 그 이미지에 대한 DELETE 리퀘스트를 보낸다면 이런 상태 코드를 보게될 수도 있습니다. |
|                                                | 413 Payload Too Large     | 현재 리퀘스트의 바디에 들어있는 데이터의 용량이 지나치게 커서 서버가 거부한다는 뜻입니다.                                                                                                                                                |
|                                                | 429 Too Many Requests     | 일정 시간 동안 클라이언트가 지나치게 많은 리퀘스트를 보냈다는 뜻입니다. 서버는 수많은 클라이언트들의 리퀘스트를 정상적으로 처리해야 하기 때문에 특정 클라이언트에게만 특혜를 줄 수는 없습니다. 따라서 지나치게 리퀘스트를 많이 보내는 클라이언트에게는 이런 상태 코드를 담은 리스폰스를 보낼 수도 있습니다.                         |
| 500<br>(서버 쪽의 문제로 인해 리퀘스트를 정상적으로 처리할 수 없음)     | 500 Internal Server Error | 현재 알 수 없는 서버 내의 에러로 인해 리퀘스트를 처리할 수 없다는 뜻입니다.                                                                                                                                                      |
|                                                | 503 Service Unavailable   | 현재 서버 점검 중이거나, 트래픽 폭주 등으로 인해 서비스를 제공할 수 없다는 뜻입니다.                                                                                                                                                 |

## <mark style="background: #ABF7F7A6;">Content-Type 헤더</mark>

현재 request 혹은 response의 body 에 들어 있는 데이터가 어떤 타입인지를 나타낸다.

| 카테고리        | 이름                                | 설명                                                                                                                        |
| ----------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| text        | text/plain                        | 일반 텍스트                                                                                                                    |
|             | text/css                          | CSS                                                                                                                       |
|             | text/html                         | HTML                                                                                                                      |
|             | text/javascript                   | JS                                                                                                                        |
| image       | image/bmp                         | bmp 이미지                                                                                                                   |
|             | image/gif                         | gif 이미지                                                                                                                   |
|             | image/png                         | png 이미지                                                                                                                   |
| audio       | audio/mp4                         | mp4 오디오                                                                                                                   |
|             | audio/ogg                         | ogg 오디오                                                                                                                   |
| video       | video/ mp4                        | mp4 비디오                                                                                                                   |
|             | video/ H264                       | H264 비디오                                                                                                                  |
| application | application/json                  | JSON 데이터                                                                                                                  |
|             | application/octet-stream          | 확인되지 않은 바이너리 파일 (텍스트 파일 이외의 파일들을 바이너리 라고 하는데 이 중에서도 특정 확장자(.png, .mp4 등)의 포맷에 해당하지 않는 데이터, 보통 이 상황에서 다운로드 받으시겠냐고 alert함.) |
|             | application/xml                   | xml 문법을 따르되 특수한 규칙을 더해 만든 데이터 타입들까지 포함해서 xml 데이터                                                                          |
|             | application/x-www-form-urlencoded | html로 데이터를 바디에 담아서 request를 보낼 때 쓰이는 헤더<br>ex) id=6&name=Jason&age=34&department=engineering (URL Query)                  |
| 통합          | multipart/form-data               |                                                                                                                           |
Content-Type 헤더에 적절한 값이 들어가 있다면 불필요한 비용 없이 쉽게 데이터의 타입을 확인할 수 있다.

## <mark style="background: #ADCCFFA6;">JSON 파일 Content-Type으로 지정</mark>

```javascript
const newMember = {
  name: 'Jerry',
  email: 'jerry@codeit.kr',
  department: 'engineering',
};

fetch('https://learn.codeit.kr/api/members', {
  method: 'POST',
  headers: { // 추가된 부분
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(newMember),
})
  .then((response) => response.text())
  .then((result) => { console.log(result); });
```

## <mark style="background: #ADCCFFA6;">XML</mark>

JSON 이전에 흔했던 데이터 포맷, 태그를 사용해서 데이터를 나타낸다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<person>
    <name>Michael Kim</name>
    <height>180</height>
    <weight>70</weight>
    <hobbies>
        <value>Basketball</value>
        <value>Listening to music</value>
    </hobbies>
</person>
```

Schema라는 별도의 문서와 함께 사용하고 각 조직, 기관 등에서 XML로 데이터를 나타낼 때 어떤 태그를 사용할 수 있다.
그곳에는 각 태그의 의미는 무엇이며, 특정 태그는 어떤 타입의 값을 가질 수 있는지 등의 정보가 담겨 있다. (validity 검증에 특화된 데이터 포맷)

## <mark style="background: #D2B3FFA6;">form tag에서 사용되는 타입</mark>

```html
...
<body>
  <div id="signup">
    <p id="title">CodeitShopping</p> 
    <form action="/upload" method="get" enctype="application/x-www-urlencoded">
      <div>
        <div><span class="label">email</span></div>
        <input class="input" type="text" id="email" name="email">
      </div>
      <div>
        <div><span class="label">password</span></div>
        <input class="input" type="password" id="password" name="password">
      </div>
      <div>
        <div><span class="label">nickname</span></div>
        <input class="input" type="text" id="nickname" name="nickname">
      </div>
      <div>
        <input id="submit-btn" type="submit" value="Sign Up">
      </div>
    </form>
  </div>
</body> 
...
```

`method="get" enctype="application/x-www-urlencoded"` 로 인해서 request시 메소드를 GET으로 설정하고 사용자가 입력한 데이터를 URL Query 부분에 `application/x-www-urlencoded` 타입으로 넣는다.
![[Pasted image 20240402015813.png]]

위의 사진이 그 결과인데, 빨간 부분을 분석해보자.

| 입력했던 실제 글자 | 표시된 내용 |
| ---------- | ------ |
| @          | %40    |
| !          | %21    |
| 공백         | +      |

이것이 `application/x-www-form-urlencoded` 타입의 특징, URL encoding이라는 작업을 수행한 결과이다.

특정 특수문자, 영어, 숫자를 제외한 다른 나라의 문자들을 이런 식으로 변환하는 것

## <mark style="background: #D2B3FFA6;">Percent Encoding</mark>

URL 처리시 Percent Encoding이라는 것이 필요하다.

| :   | /   | ?   | #   | [   | ]   | @   | !   | $   | &   | '   | ... | ' '(공백)  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | -------- |
| %3A | %2F | %3F | %23 | %5B | %5D | %40 | %21 | %24 | %26 | %27 | ... | %20 또는 + |
URL 에서 특별한 용도를 갖고 있는 문자들이 다른 용도로 사용되면 변환이 필요하다.

또한 한국어, 중국어, 아랍어 등도 이것이 필요하다.

## <mark style="background: #D2B3FFA6;">Percent Encoding 규칙</mark>

	먼저 UTF-8로 문자를 인코딩 > 한 바이트 당 그 앞에 % (한글은 3 바이트) 
	(ex) 코딩 > %EC%BD%94%EB%94%A9)

## <mark style="background: #D2B3FFA6;">application/x-www-form-urlencoded 와  url encoding의 관계</mark>

후자의 원리(percent encoding)를 그대로 반영한 데이터 타입, 그래서 이름도 urlencoded이다.

method를 GET > POST로 바꾸면 해당 query부분을 바디에 넣을 수도 있다.

![[Pasted image 20240402021012.png]]

## <mark style="background: #D2B3FFA6;">application/x-www-form-urlencoded 실무 적용</mark>

```javascript
const urlencoded = new URLSearchParams();
urlencoded.append('email', 'tommy@codeit.kr');
urlencoded.append('password', 'codeit123!');
urlencoded.append('nickname', 'Nice Guy!');

fetch('https://learn.codeit.kr/api/members', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded',
  },
  body: urlencoded,
})
  .then((response) => response.text())
  .then((result) => {
    console.log(result);
  });
```

## <mark style="background: #ADCCFFA6;">multipart/form-data</mark>
여러 종류의 데이터를 하나로 합친 데이터를 의미하는 타입

![[Pasted image 20240402021910.png]]

위의 예시를 보면 이미지와 text 모든걸 한번에 데이터에 담아서 보내야 한다.

이때 사용되는 데이터 타입이 `multipart/form-data`이다.

## <mark style="background: #D2B3FFA6;">HTML로 다루는 법</mark>

```html
...
<body>
  <div id="signup">
    <p id="title">CodeitShopping</p> 
    <form action="/upload" method="post" enctype="multipart/form-data">
      <div>
        <input id="image" type="file" name="file" accept="image/*">
        <div id="profile">
          <div id="plus">+</div>
        </div>
       </div>
      <div>
        <div><span class="label">email</span></div>
        <input class="input" type="text" id="email" name="email">
      </div>
      <div>
        <div><span class="label">password</span></div>
        <input class="input" type="password" id="password" name="password">
      </div>
      <div>
        <div><span class="label">nickname</span></div>
        <input class="input" type="text" id="nickname" name="nickname">
      </div>
      <div>
        <input id="submit-btn" type="submit" value="Sign Up">
      </div>
    </form>
  </div>
</body>
... 
```

## <mark style="background: #D2B3FFA6;">JS로 다루는 법</mark>

```javascript
...
const formData = new FormData();
formData.append('email', email.value);
formData.append('password', password.value);
formData.append('nickname', nickname.value);
formData.append('profile', image.files[0], "me.png");

fetch('https://learn.codeit.kr/api/members', {
  method: 'POST',
  body: formData,
})
  .then((response) => response.text())
  .then((result) => { console.log(result); });
```

## <mark style="background: #D2B3FFA6;">multipart/form-data 개발자 도구 활용</mark>

![[Pasted image 20240402022804.png]]

개발자 도구에서 이 데이터 타입을 확인해볼 수 있다.
뒤쪽 boundary부분은 말그대로 boundary로 각 데이터를 나눠주는 경계선이라고 생각하면 된다. body부분에서 각 데이터가 이 문자열로 나누어져 있다. 확인해보자.

![[Pasted image 20240402022814.png]]

view source 버튼으로 확인한 상세 정보이다.
잘 나눠보자.

![[Pasted image 20240402022823.png]]

보이는 것과 같이 name에 각 데이터가 들어가 있고 아까 본 문자열로 나뉘어져 있다.

4번의 이미지의 경우 다른 것과 같이 마지막 줄에 value가 나타나는데 (이미지의 바이너리 데이터) 개발자 도구에서 안 보여주는 것으로 추정

## <mark style="background: #BBFABBA6;">Body</mark>

JSON 데이터가 들어가는 부분

## <mark style="background: #ABF7F7A6;">fetch</mark>

서버에 request를 보내고 response를 받는 함수

```javascript
fetch('url')
	.then ((response)=> {/*functionbody*/}) // callback function
	.then ((result)=>{ functionbody;});
```

## <mark style="background: #ADCCFFA6;">Option Object</mark>

```javascript
fetch('url',{method: httpmethodName, body: JSON.stringify(dataName)}/*option object*/)
	.then ((response)=> {/*functionbody*/}) // callback function
	.then ((result)=>{ functionbody;});
```

method에 http method 를 추가한다. (Default : GET)

body에 함수 본문을 적는다. (JSON 포맷, 해당 본문은 개발자 도구의 Network, Request payload를 보면 알 수 있다.)

## <mark style="background: #ADCCFFA6;">response Object</mark>

response에 대한 모든 부가 정보들, 실제 내용을 담고 있는 하나의 객체

`response.text()`처럼 필터링을 거쳐야 then으로 데이터를 활용할 수 있다. (response.text()는 Promise를 리턴한다)

## <mark style="background: #ADCCFFA6;">Promise.then</mark>

response가 왔을 때 arg에 있는 function을 실행하는 callback function

이 함수의 리턴 값은 다음 then이 받아서 사용할 수 있다.

## <mark style="background: #ABF7F7A6;">JSON</mark>

서버와 데이터가 통신할 때 사용되는 데이터 포맷

## <mark style="background: #ADCCFFA6;">제한 조건</mark>

1. Property 이름은 반드시 "" 안에 존재
2. property의 value가 문자열일 경우 "" 안에 존재
3. `undefined` `NaN` `Infinity` 등 사용할 수 없는 Property value가 존재
4. comment 추가 불가능

## <mark style="background: #ADCCFFA6;">JSON관련 메소드</mark>

## <mark style="background: #D2B3FFA6;">JSON 해제 (Deserialization)</mark>

```javascript
const Data = JSON.parse(JSONData);
```

## <mark style="background: #D2B3FFA6;">JSON 만들기 (serialization)</mark>

```javascript
const JSONData = JSON.stringify(Data);
```

## <mark style="background: #D2B3FFA6;">response객체에서 JSON으로 바로 받기</mark>

JSON이 아닐 경우 에러가 발생하니 주의

```javascript
fetch('url')
	.then((response)=>{response.json()})
	.then((result)=>{console.log(result)}) // json이 객체 형식으로 리턴
```

## <mark style="background: #ADCCFFA6;">JSON 데이터 포맷</mark>

serialization 된 JSON 데이터는 string 타입이며 serialization 되기 전의 데이터가 기본으로 내장했던 프로퍼티들을잘라내고 순수 데이터 만을 저장하고 있다.

Deserialization 된 데이터는 JS Object이며 JS Object처럼 똑같이 Property에 접근할 수 있다.

## <mark style="background: #FFB86CA6;">Web API</mark>

개발할 때 사용할 수 있도록 특정 라이브러리나 플랫폼 등이 제공하는 데이터나 함수 등

## <mark style="background: #FFF3A3A6;">가이드 라인 : REST API</mark>

REST = Representational State Transfer

## <mark style="background: #BBFABBA6;">조건 (Rest Architecture)</mark>

1. <mark style="background: #ABF7F7A6;">Client-Server</mark>
	1. Client와 Server가 서로 문제를 완벽하게 나눠야함.
2. <mark style="background: #ABF7F7A6;">Stateless</mark>
	1. 각 리퀘스트는 Stateless이다. 고로 어떤 context도 저장하지 않는다.
	2. 각 리퀘스트는 모든 필요한 정보를 담아서 보내야 한다.
3. <mark style="background: #ABF7F7A6;">Cache</mark>
	1. 네트워크 비용 절감을 위해서 필요
	2. Server가 캐시 사용 여부를 결정해서 보내야 한다.
4. <mark style="background: #ABF7F7A6;">Uniform Interface</mark>
	1. identification of resources
		1. resource가 URI(Uniform Resource Identifier)로 식별할 수 있어야 한다.
		2. URL은 리소스를 나타내기 위해서만 사용하고, 리소스에 대한 처리는 메소드로 표현해야 한다.
		3. Document는 단수 명사, Collection은 복수 명사로 표시
	2. manipulation of resources through representations
		1. 사용되는 resource는 원본이 아니라 그것의 재현(모방)품이다.
	3. self-descriptive messages
		1. Client의 Request나 Server의 Response는 모두 그 자체의 정보만으로 그 자체의 모든 것을 해석할 수 있어야 한다.
	4. hypermedia as the engine of application state
		1. Hypermedia : Hypertext(서로 연결된 문서) 보다 더 큰 개념이다 (이미지, 소리, 영상 등 까지도 모두 포괄하는 더 넓은 개념의 단어) Web은 Distributed Hypermedia System
		2. Server Response에는 현재 상태에서 다른 상태로 이전할 수 있는 링크를 포함하고 있어야 한다.
5. <mark style="background: #ABF7F7A6;">Layered System</mark>
	1. proxy, gateway 같은 중간 매개 요소를 두고 보안, 로드 밸런싱 등을 수행할 수 있어야 한다.
	2. Client, server 사이에는 hierarchical layers 형성
6. <mark style="background: #ABF7F7A6;">Code on Demand</mark>
	1. Server로부터 바로 실행할 수 있는 script나 applet 파일을 받을 수 있어야 한다.

## <mark style="background: #FFB86CA6;">Ajax (Asynchronous Javascript And Xml)</mark>

현재 페이지를 그대로 유지한 채로 서버에 request, response 작업을 해서 load하지 않고 변화를 주는 기술

비동기적으로 request, response를 하는데 기반이 되는 기술들의 집합 (옛날에 만들어진 거라 XML이 끼어있음.)

![[Pasted image 20240402023835.png]]
이런 지도앱에서 정보를 보기 위해 클릭할 경우, 페이지가 로드되지 않고 정보창만 떠오르는 것이 Ajax 통신의 대표적인 예시이다.

예전 JS에서는 XMLHttpRequest라는 객체를 통해 Ajax 통신을 할 수 있다.

```javascript
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://learn.codeit.kr/api/members');
xhr.onload = function () {
  console.log(xhr.response);
};
xhr.onerror = function () {
  alert('Error!');
};
xhr.send();
```

## <mark style="background: #FFF3A3A6;">Ajax 실무 활용</mark>

## <mark style="background: #BBFABBA6;">fetch 활용</mark>

```javascript
// (위 예시를 단순화한 코드입니다)
function getLocationInfo(latitude, longitude) {
  fetch('https://map.google.com/location/info?lat=latitude&lng=longitude')
    .then((response) => response.text())
    .then((result) => { /* 사용자 화면에 해당 위치 관련 정보 띄워주기 */ });
}
```

## <mark style="background: #BBFABBA6;">axios package</mark>

## <mark style="background: #FFB86CA6;">웹 이외의 통신</mark>

HTTP, HTTPS 이외에 다양한 프로토콜들이 존재하는데 이 프로토콜 들은 각각 네트워크 통신의 특정 계층에 속한다.

![[Pasted image 20240402025426.png]]

위일수록 고수준 프로토콜 반대는 저수준 프로토콜이라고 한다.

TCP같은 저수준 프로토콜은 게임 서버 개발, IOT 개발에서 사용되기도 한다.