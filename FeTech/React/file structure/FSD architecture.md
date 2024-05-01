Feature- Sliced Design

## 세가지 구성

![[Pasted image 20240412130509.png]]
## layer

각 layer는 아래 있는 layer들과만 interact할 수 있다.( import, export )

| 이름       | 설명                                                                    |
| -------- | --------------------------------------------------------------------- |
| shared   | 기능적으로 재사용가능하며 project의 specifics와 떨어져 있음. (segments를 바로 가진다는 특징이 있다.) |
| entities | 비즈니스에 관련된 entities (user, product, order)                             |
| features | 유저와 상호작용, 유저에게 비즈니스적 가치를 줌 (SendComment, AddToCart, UsersSearch)      |
| widgets  | feature와 entity를 합친 기능적인 블록 (IssuesList, UserProfile)                 |
| pages    | entity, feature, widget을 합친 하나의 페이지                                   |
| app      | 전체 setting, style, provider                                           |

## slice

비즈니스 도메인에 따라서 코드를 나눈 것, 

코드베이스를 논리적으로 관련된 모듈을 서로 가까이함으로써 찾기 쉽게 만든다.

slice는 slice를 사용하지 않는다. high cohesion, low coupling

각 slice는 Segment를 갖고 있다.

## segment

기술적 목적으로 slice 안의 코드를 나눠놓은 것

`ui`, `model`, `api`, `lib` 등이 있다.

무엇인지가 아니라 왜 있는지를 중점으로 이름을 지어야 한다.

## 과정

1. 페이지를 나눈다.
	1. 각 페이지를 page layer의 slice로 둔다. 폴더 안에 폴더가 들어가 있음 지금
2. 페이지의 내부 요소를 내눈다.
	1. 내부 요소에 대한 segment를 만든다.
	2. ui, api, model 등등이 있음.
3. 개발하면서 shared에 넣을걸 빼둔다.
4. 엄격한 public API를 정의하라.
	1. public API : 다른 모듈에서 뭘 받을 수 있는지 정의하는 slice, segment를 의미
		1. index.js 같은 re-exporting objects
		2. shared같은 segment만 존재하는 경우는 각각 만들고
		3. slice가 존재하면 해당 slice 하나의 index.jsx에 segment를 모아라.
	2. ex) export {header} from '../ui/header'만 있는 파일이 ui폴더에 있어서 import 하기 쉽게 만드는 용도
5. ui에서 가장 흔히 사용되는 블록을 찾아라.

## 프로젝트 중 안 팁

scss는 jsx에서 import로 바로 받아서 쓸 수 있다.
