## 값을 표시할 태그 구분을 구분할 때 비표준 속성

```javascript
const new = {};
```

```javascript
const fields = document.querySelectorAll('[field]');
const task = {
  title: '코드 에디터 개발',
  manager: 'CastleRing, Raccoon Lee',
  status: '',
};
for (let tag of fields) {
  const field = tag.getAttribute('field');
  tag.textContent = task[field];
}
```

### 스타일이나 데이터 변경에 활용

```javascript
const fields = document.querySelectorAll('[field]');
const task = {
  title: '코드 에디터 개발',
  manager: 'CastleRing, Raccoon Lee',
  status: '',
};

for (let tag of fields) {
  const field = tag.getAttribute('field');
  tag.textContent = task[field];
}

const btns = document.querySelectorAll('.btn');
for (let btn of btns) {
  const status = btn.getAttribute('status');
  btn.onclick = function () {
    fields[2].textContent = status;
    fields[2].setAttribute('status', status);
  };
}
```

### dataset property

```html
<!DOCTYPE html>
<html lang="ko">

<head>
  <meta charset="UTF-8">
  <title>JS with Codeit</title>
</head>

<body>
  <p>할 일 : <b data-field="title"></b></p>
  <p>담당자 : <b data-field="manager"></b></p>
  <p>상태 : <b data-field="status"></b></p>
  <div>
    상태 변경: 
    <button class="btn" data-status="대기중">대기중</button>
    <button class="btn" data-status="진행중">진행중</button>
    <button class="btn" data-status="완료">완료</button>
  </div>
  <script src="index.js"></script>
</body>
</html>
```

```css
[data-status] {
  padding: 5px 10px;
}
[data-status="대기중"] {
  background-color: #FF6767;
  color: #FFFFFF;
}
[data-status="진행중"] {
  background-color: #5f62ff;
  color: #FFFFFF;
}
[data-status="완료"] {
  background-color: #07c456;
  color: #FFFFFF;
}
```

```javascript
const fields = document.querySelectorAll('[data-field]');
const task = {
  title: '코드 에디터 개발',
  manager: 'CastleRing, Raccoon Lee',
  status: '',
};
for (let tag of fields) {
  const field = tag.dataset.field;
  tag.textContent = task[field];
}
const btns = document.querySelectorAll('.btn');
for (let btn of btns) {
  const status = btn.dataset.status;
  btn.onclick = function () {
    fields[2].textContent = status;
    fields[2].dataset.status = status;
  };
}
```


# 마우스 이벤트

|이벤트 타입|설명|
|---|---|
|`mousedown`|마우스 버튼을 누르는 순간|
|`mouseup`|마우스 버튼을 눌렀다 떼는 순간|
|`click`|왼쪽 버튼을 클릭한 순간|
|`dblclick`|왼쪽 버튼을 빠르게 두 번 클릭한 순간|
|`contextmenu`|오른쪽 버튼을 클릭한 순간|
|`mousemove`|마우스를 움직이는 순간|
|`mouseover`|마우스 포인터가 요소 위로 올라온 순간|
|`mouseout`|마우스 포인터가 요소에서 벗어나는 순간|
|`mouseenter`|마우스 포인터가 요소 위로 올라온 순간 (버블링이 일어나지 않음)|
|`mouseleave`|마우스 포인터가 요소에서 벗어나는 순간 (버블링이 일어나지 않음)|

# 키보드 이벤트

|이벤트 타입|설명|
|---|---|
|`keydown`|키보드의 버튼을 누르는 순간|
|`keypress`|키보드의 버튼을 누르는 순간 ('a', '5' 등 출력이 가능한 키에서만 동작하며, Shift, Esc 등의 키에는 반응하지 않음)|
|`keyup`|키보드의 버튼을 눌렀다 떼는 순간|

# 포커스 이벤트

|이벤트 타입|설명|
|---|---|
|`focusin`|요소에 포커스가 되는 순간|
|`focusout`|요소로부터 포커스가 빠져나가는 순간|
|`focus`|요소에 포커스가 되는 순간 (버블링이 일어나지 않음)|
|`blur`|요소로부터 포커스가 빠져나가는 순간 (버블링이 일어나지 않음)|

# 입력 이벤트

|이벤트 타입|설명|
|---|---|
|`change`|입력된 값이 바뀌는 순간|
|`input`|값이 입력되는 순간|
|`select`|입력 양식의 하나가 선택되는 순간|
|`submit`|폼을 전송하는 순간|

# 스크롤 이벤트

|이벤트 타입|설명|
|---|---|
|`scroll`|스크롤 바가 움직일 때|

# 윈도우 창 이벤트

|이벤트 타입|설명|
|---|---|
|`resize`|윈도우 사이즈를 움직일 때 발생|

# 이벤트 객체

# 1.  공통 프로퍼티

|프로퍼티|설명|
|---|---|
|`type`|이벤트 이름 ('click', 'mouseup', 'keydown' 등)|
|`target`|이벤트가 발생한 요소|
|`currentTarget`|이벤트 핸들러가 등록된 요소|
|`timeStamp`|이벤트 발생 시각(페이지가 로드된 이후부터 경과한 밀리초)|
|`bubbles`|버블링 단계인지를 판단하는 값|


## 캡쳐링

<img src="https://bakey-api.codeit.kr/api/files/resource?root=static&seqId=3807&directory=%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%8E%E1%85%A7%E1%84%85%E1%85%B5%E1%86%BC.png&name=%E1%84%8F%E1%85%A2%E1%86%B8%E1%84%8E%E1%85%A7%E1%84%85%E1%85%B5%E1%86%BC.png" width=700px/>

## <mark style="background: #FFF3A3A6;">Mouse Event</mark>
## <mark style="background: #BBFABBA6;">MouseEvent property</mark>

| 프로퍼티               | 설명                                                  |
| ------------------ | --------------------------------------------------- |
| `button`           | 누른 마우스의 버튼 (0: 왼쪽, 1: 가운데(휠), 2: 오른쪽)               |
| `clientX, clientY` | 마우스 커서의 브라우저 표시 영역에서의 위치                            |
| `pageX, pageY`     | 마우스 커서의 문서 영역에서의 위치                                 |
| `offsetX, offsetY` | 마우스 커서의 이벤트 발생한 요소에서의 위치                            |
| `screenX, screenY` | 마우스 커서의 모니터 화면 영역에서의 위치                             |
| `altKey`           | 이벤트가 발생할 때 alt키를 눌렀는지                               |
| `ctrlKey`          | 이벤트가 발생할 때 ctrl키를 눌렀는지                              |
| `shiftKey`         | 이벤트가 발생할 때 shift키를 눌렀는지                             |
| `metaKey`          | 이벤트가 발생할 때 meta키를 눌렀는지 (window는 window키, mac은 cmd키) |

## <mark style="background: #BBFABBA6;">MouseEvent.type</mark>

## <mark style="background: #ABF7F7A6;">MouseEvent.button</mark>

	0 1 2 순서로 버튼 유형

## <mark style="background: #ABF7F7A6;">MouseEvent.type</mark>

| 이벤트 이름      | 설명                        |
| ----------- | ------------------------- |
| mousedown   | 마우스 누름                    |
| mouseup     | 마우스 뗌                     |
| mousemove   | 마우스 움직임                   |
| mouseover   | 마우스 포인터 요소 밖 이동           |
| mouseout    | 마우스 포인터 요소 안 이동           |
| mouseenter  | 마우스 포인터가 요소 바깥에서 안쪽으로 들어감 |
| mouseleave  | 마우스 포인터가 요소 안쪽에서 바깥으로 나감. |
| contextmenu | 오른쪽 버튼 메뉴                 |
| click       | 클릭                        |
| dbclick     | 두번 클릭                     |

## <mark style="background: #ADCCFFA6;">click시</mark>

	mousedown > mouseup > click	

## <mark style="background: #ADCCFFA6;">dbclick시</mark>
	
	mousedown > mouseup > click >mousedown > mouseup > click > dbclick

## <mark style="background: #ADCCFFA6;">mouseenter / mouseleave  와 mouseover / mouseout 차이점
</mark>

* 버블링
	* mouseenter / mouseleave는 버블링이 일어나지 않는다.
		* Event propagation이 일어나지 않는다.
	* mouseover / mouseout은 버블링이 일어난다.
		* Event propagation이 일어나서 부모 요소도 Event의 영향을 받는다.
* 자식 요소의 영역을 계산
	* mouseenter / mouseleave 는 자식 요소의 영역을 계산하지 않는다.
	* mouseover / mouseout 는 자식 요소의 영역을 계산한다.
		* 자식요소에서 부모요소로 가는 것조차 count 1 로 생각한다.

![[eventhandler1.mkv]]

## <mark style="background: #ABF7F7A6;">좌표</mark>

<img src="https://bakey-api.codeit.kr/api/files/resource?root=static&seqId=3817&directory=clientpageoffset&name=clientpageoffset" width=700px/>

## <mark style="background: #ADCCFFA6;">화면 기준</mark>

| 이벤트 이름             | 설명  |
| ------------------ | --- |
| MouseEvent.clientx |     |
| MouseEvent.clienty |     |
## <mark style="background: #ADCCFFA6;">target 기준</mark>

| 이벤트 이름             | 설명  |
| :----------------- | --- |
| MouseEvent.offsetx |     |
| MouseEvent.offsety |     |
## <mark style="background: #ADCCFFA6;">웹문서 기준</mark>

| 이벤트 이름           | 설명  |
| ---------------- | --- |
| MouseEvent.pagex |     |
| MouseEvent.pagey |     |

## <mark style="background: #ABF7F7A6;">target</mark>

| 이벤트 이름                   | 설명                  |
| ------------------------ | ------------------- |
| MouseEvent.target        | 이벤트가 발생한 요소         |
| MouseEvent.relatedTarget | 이벤트 발생 직전 마우스 위치 요소 |

## <mark style="background: #FFF3A3A6;">Keyboard Event</mark>

## <mark style="background: #BBFABBA6;">KeyboardEvent Property</mark>

| 프로퍼티       | 설명                                                  |
| ---------- | --------------------------------------------------- |
| `key`      | 누른 키가 가지고 있는 값                                      |
| `code`     | 누른 키의 물리적인 위치                                       |
| `altKey`   | 이벤트가 발생할 때 alt키를 눌렀는지                               |
| `ctrlKey`  | 이벤트가 발생할 때 ctrl키를 눌렀는지                              |
| `shiftKey` | 이벤트가 발생할 때 shift키를 눌렀는지                             |
| `metaKey`  | 이벤트가 발생할 때 meta키를 눌렀는지 (window는 window키, mac은 cmd키) 

## <mark style="background: #BBFABBA6;">KeyboardEvent.type</mark>

| 키보드 이름   | 설명                          |
| -------- | --------------------------- |
| keydown  | 키보드 버튼 순간 (웹 표준에서 권장하는 이벤트) |
| keypress | 키보드 버튼을 누른 순간               |
| keyup    | 키보드 버튼을 눌렀다 뗀 순간            |
## <mark style="background: #ABF7F7A6;">keypress</mark>

	keydown > keypress > keyup

만약 계속 누르고 있다면

	keydown (반복)

shift같은 modifier key들은 취급하지 않는다.

웹표준에서는 keypress event보다는 keydown이벤트를 권장한다.
## <mark style="background: #BBFABBA6;">KeyboardEvent.key</mark>

이벤트가 발생한 버튼의 값

## <mark style="background: #BBFABBA6;">KeyboardEvent.code</mark>

이벤트가 발생한 버튼의 키보드에서 물리적인 위치 
(shift인데 오른쪽인지 왼쪽인지 같은 구체적인 정보 저장)

## <mark style="background: #FFF3A3A6;">Input Tag 관련 Event</mark>
## <mark style="background: #BBFABBA6;">Focus Event</mark>

| 이벤트 이름   | 설명                      |
| -------- | ----------------------- |
| focusin  | 요소에 포커스가 되었을 때          |
| focusout | 요소에 포커스가 빠져나갈 때         |
| focus    | 요소에 포커스가 되었을 때 (버블링 x)  |
| blur     | 요소에 포커스가 빠져나갈 때 (버블링 x) |

## <mark style="background: #BBFABBA6;">Input Event</mark>

| 이벤트 이름 | 설명                                                                           |
| ------ | ---------------------------------------------------------------------------- |
| input  | 사용자가 입력을 할 때                                                                 |
| change | 요소의 값이 변했을 때<br>(입력 후에 값이 변했는데 포커스가 이동했을 경우, enter를 눌렀을 경우(focus를 잃진 않음) 등등) |

