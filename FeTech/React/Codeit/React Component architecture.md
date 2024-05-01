## <mark style="background: #FF5582A6;">관심사 분리</mark>

## <mark style="background: #FFB86CA6;">수평적인 분리</mark>

![[Pasted image 20240427122512.png]]

1. Presentation Layer: 보이는 요소인 HTML, CSS, UI에 필요한 상태관리로 이루어진 UI 컴포넌트

2. Business Layer: 비즈니스 로직과 정책 등에 관련된 컴포넌트

3. Resource Access Layer: 유저에게 제공할 데이터를 받기 위해 백엔드 서버에 요청, 로컬 스토리지 접근, 외부 api 서버 요청 등 외부 정보 접근과 관련된 훅 또는 함수

## <mark style="background: #FFB86CA6;">수직적인 분리</mark>

![[Pasted image 20240427122752.png]]

동일한 도메인을 모듈로 묶어서 분리 

![[Pasted image 20240427122907.png]]

도메인 별로 이런식으로 구조를 갖고 있을 수 있다.

## <mark style="background: #FFB86CA6;">React Component에 관심사의 분리 적용</mark>

## <mark style="background: #FFF3A3A6;">수평적 분리</mark>

## <mark style="background: #BBFABBA6;">Presentational & container pattern (Smart and Dumb pattern)</mark>

## <mark style="background: #ABF7F7A6;">Presentaional Component</mark>

사용자가 보고, 조작하는 UI 컴포넌트

UI 만을 위한 상태를 제외하고는 상태를 가지지 않고 Container 컴포넌트가 내려준 props를 통해 조작됩니다

## <mark style="background: #ABF7F7A6;">Container Component</mark>

데이터를 받거나 비즈니스 로직을 설정

UI 컴포넌트를 포함할 수 있고, UI 컴포넌트에 props를 전달해 UI를 조작

## <mark style="background: #ABF7F7A6;">데이터 접근 분리</mark>

컴포넌트에 데이터를 받아오는 부분이 있는 경우가 많은데 이 데이터 접근 로직만 커스텀 훅으로 빼면 재사용 가능

## <mark style="background: #ABF7F7A6;">유틸리티 함수 분리</mark>

utility function : 계산과 처리를 대신하는 일반 함수

- 데이터 접근을 제외한 Container 컴포넌트 → feature
- 데이터 접근 → data-access
- UI 컴포넌트 → ui
- 유틸리티 함수 → util

이러한 카테고리들을 다음과 같이 의존성을 정리해서 사용할 수 있다.

- feature: feature, ui, data-access, util 모두에 의존할 수 있습니다.
- data-access: data-access, util에 의존할 수 있습니다.
- ui: ui, util에 의존할 수 있습니다.
- util: util 끼리만 의존할 수 있습니다.

## <mark style="background: #FFF3A3A6;">수직적 분리</mark>

비즈니스 도메인에 따라 나누면 좋다.

## <mark style="background: #FFF3A3A6;">결론</mark>

규모가 클수록 더 필요하고 작을수록 필요하지 않다. 재사용성이 낮거나 일회용 페이지를 작성할때는 알아서 하면 된다.