# CSS 박스 모델

 HTML 요소 박스의 크기는 콘텐츠 크기에 안쪽 여백인 padding과 테두리인 border 그리고 바깥쪽 여백인 margin에 의해 결정된다.

CSS 속성의 종류는 크게 두 가지로 나뉜다.

1. 상속되는 속성

   - visibility

   

2. 상속되지 않는 속성

   - display
   - padding
   - border
   - margin
   - width
   - height
   - min–width, min–height, max–width, max–height 

   - overflow

<br>

<br>

## 상속되는 속성

### visibility 속성

- visibility는 HTML 요소에 의해 만들어진 박스의 화면상 표시 여부를 지정

- 기본 값 : `visible`

- visibility 속성 값을 `hidden`으로 지정할 경우에는 요소를 화면에서 숨김

  `display : none`과 달리 요소의 원래 영역은 유지하게 됨

- `collapse`는 테이블 내부 객체로, 테이블의 행, 행 그룹, 열, 열 그룹에 적용되며 `hidden`과 비슷함

```css
div { visibility : hidden ; }
```

<br>

<br>

## 상속되지 않는 속성

### display 속성

- HTML 요소의 표현 방식을 지정
  - HTML 요소는 각각 기본으로 지정된 다양한 display 값이 존재함

- 기본값 : `inline`

- `display : inline`

  - 자신의 전후로 줄바꿈을 만들지 않으며,

    포함하는 요소가 너비에 의해 자동 줄바꿈됨

  - 요소의 폭은 콘텐츠에 따라 결정됨

  - width 속성으로 요소의 너비를 직접 지정할 수 없음

- `display : block`

  - 기본적으로 부모 요소의 너비 전체를 채움
  - width 속성으로 요소의 너비를 직접 지정할 수 있음
  - 요소 박스 전후로 줄바꿈이 생김

- `display : inline-block`

  - 요소 전후로 줄바꿈이 생기지는 않으나,

    block 요소처럼 width 속성으로 요소의 너비를 직접 지정할 수 있음

- 기타 값

  - `none` - 화면에 요소를 표시하지 않음

  - `run-in` - 요소의 형제요소가 block 요소가 아닐 경우, "block" 요소로 정의

    ​				block 요소일 경우, "inline" 요소로 정의

  - `list-item` - 목록 요소로 보여지도록 함

  - table 관련 값 - 테이블로 표시함

  - `ruby` - 루비 요소로 표현함

<br>

### padding 속성

-  content 영역과 border 사이의 안쪽 여백을 지정
- 속성 값은 1~4개까지 입력 가능
- 입력 값의 개수에 따라 적용되는 방향이 다름

<img src="https://seulbinim.github.io/WSA/images/design/padding.png" alt="img" style="zoom:33%;" />

- 속성 값은 길이 단위 값으로, 음수 값은 사용할 수 없음
- 속성 값에 백분율을 사용할 경우, 상위 요소의 크기를 참조하여 비율로 결정(음수 값X)

```css
div { padding : 10px ; }
div { padding : 10px 20px ; }
div { padding : 10px 20px 30px ; }
div { padding : 10px 20px 30px 40px ; }
div {
 padding-top : 10px ;
 padding-right : 20px ;
 padding-bottom : 30px ;
 padding-left : 40px ;
}
```

<br>

### border 속성

- 요소 박스의 테두리를 지정
- border-[방향] 등으로 박스의 특정 위치만을 지정 가능

- 세부 속성을 사용하여 테두리 선의 모양과 굵기 및 색상 지정 가능

```css
div { border : 1px solid #ccc ; }
div { border-width : 1px ; }
div { border-style : solid ; }
div { border-color : #ccc ; }
```

<br>

### margin 속성

- border를 기준으로 다른 요소와의 바깥쪽 여백을 지정
- padding 속성과 사용 방법이 동일함

<img src="https://seulbinim.github.io/WSA/images/design/margin.png" alt="img" style="zoom:33%;" />

- padding 속성과 달리 속성 값에 음수 값을 사용할 수  있음
- margin의 특성 중에 상하로 인접한 박스의 display 속성 값이 "block"인 경우, 마진 겹침(Margin Collapsing) 현상이 발생함

<img src="https://seulbinim.github.io/WSA/images/design/margin-collapsing.png" alt="img" style="zoom: 25%;" />



```css
div { margin : 10px ; }
div { margin : 10px 20px ; }
div { margin : 10px 20px 30px ; }
div { margin : 10px 20px 30px 40px ; }
div {
 margin-top : 10px ;
 margin-right : 20px ;
 margin-bottom : 30px ;
 margin-left : 40px ;
}
```

<br>

### width, height 속성

- 요소의 너비와 높이를 지정

- display : inline; 요소에는 적용되지 않음

  - auto : 기본 값

    `display : block;` 인 경우, width 속성은 화면 너비 전체를 차지함

    height 속성은 콘텐츠 높이에 따라 자동 조절됨

  - length : 길이 단위 값(box-sizing 속성 값에 따름)

    height 속성의 경우, 콘텐츠 높이가 지정된 값보다 크더라도 요소의 높이는 변경되지 않으며, 음수 값X

  - percentage : 자신을 포함한 요소의 값을 참조하여 비율로 결정함

    height 속성의 경우, length처럼 콘텐츠 높이로 계산된 값이 지정된 값보다 크더라도 요소의 높이는 변경되지 않으며, 음수 값X

<br>

### min–width, min–height, max–width, max–height 속성

- 요소의 최소 및 최대 너비와 높이를 지정
- width 및 height 속성과 사용 값이 동일함

```css
div { min-width : 200px ; }
div { max-width : 50% ; }
div { min-height : 200px ; }
div { max-height : 50% ; }
```

<br>

### overflow 속성

- 콘텐츠가 요소의 콘텐츠 영역을 벗어나는 경우의 처리 방법을 지정
- `overflow : visible;`
  - 오버 플로우된 콘텐츠 표시
- `overflow : hidden; `
  - 오버 플로우된 콘텐츠 숨김
- `overflow : scroll;` 
  - 요소 박스에 스크롤 바 생성
- `overflow : auto;  `
  - 사용자 도구의 기본 설정 값 대로 작동하게 함
- 상세 속성인 overflow-x나 overflow-y 사용 가능

```css
div { overflow : auto; }
```

<br>

<br>

## 참고 자료

- [CSS3](https://seulbinim.github.io/WSA/box-model.html)

