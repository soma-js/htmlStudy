## Day1

### CSS 와 CSS Normalize 가 필요한 이유

크롬, Edge 등의 브라우저에는 margin 값이 8px가 기본적으로 설정되어 있습니다. 이뿐만 아니라 <table> 태그에 border-collapse의 기본값인 separate가 설정되어 있어 테두리 사이에 가녁을 두어 비교적 부자스러운 디자인이 나타납니다. 이러한 부분을 없애기 위해 2가지의 방법으로 초기화할 수 있습니다.

Reset CSS
margin, table 등 디자인이 부자여느러운 것을 없애기 위해 Reset CSS 파일이 있습니다. 이는 브라우저의 스타일을 초기화하기 위해 사용하는 파일입니다. 크로스브라우징을 구현할 때 유용합니다. 대표적인 사이트로는 cssreset가 있습니다.

Normalize CSS
Reset CSS와 마찬가지로 초기화하는 파일이지만 HTML 요소의 기본 스타일을 브라우저 간 일관성을 유지하도록 돕는 CSS 파일입니다. 대표적인 사이트로는 이곳(https://webpublishingonline.com/entry/Reset-CSS%EC%99%80-Normalize-CSS%EA%B0%80-%ED%95%84%EC%9A%94%ED%95%9C-%EC%9D%B4%EC%9C%A0#:~:text=%ED%8C%8C%EC%9D%BC%EC%9E%85%EB%8B%88%EB%8B%A4.%20%EB%8C%80%ED%91%9C%EC%A0%81%EC%9D%B8%20%EC%82%AC%EC%9D%B4%ED%8A%B8%EB%A1%9C%EB%8A%94-,%EC%9D%B4%EA%B3%B3,-%EC%97%90%EC%84%9C%20%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C%ED%95%A0%20%EC%88%98)에서 다운로드할 수 있습니다.

Reset CSS, Normalize CSS가 필요한 가장 큰 이유는 크로스브라우징을 위해서입니다. 브라우저마다 HTML 시맨틱 태그에 대한 기본값이 다르므로 그 차이를 없애는 데 필요합니다.

크로스브라우징은 웹 표준에서 중요시하는 기술로 크롬, 사파리, 파이어폭스 등의 브라우저에서 웹 사이트를 접속했을 때 어느 한쪽에 최적화되어 치우치지 않도록 공통 요소를 사용하여 제작하는 것을 말합니다.

### IR 기법

Image Replacement - IR기법은 스크린 리더 사용자들을 위한, 이미지를 대신하는 대체 텍스트를 제공하는 기법이다.

특정 요소를 스크린 리더 친화적 환경을 위해서라면 html 문서에는 남겨둬야하지만, 동시에 디자인적으로는 숨겨야 하는 때가 있다. 주로 헤딩(h1, h2, ...) 태그나 input의 label과 같이 정보를 전달하는 텍스트 등이 주로 그렇다.

이런 IR 기법은 스크린 리더의 사용성을 높일 뿐 아니라 검색 엔진 최적화에도 도움을 준다.

요소를 그저 "보이지 않게" 하려면 여러 방법들이 쉽게 떠오른다.

color: transparent;
opacity: 0;
visiblity: hidden;
display: none; 등등 ...
하지만, 이런 속성들은 자신의 공간을 그대로 차지한다거나, 아예 마크업 자체를 인식하지 못한다거나 하는 문제로 인해 위 명시한 IR 기법의 목적을 위해 사용하기에는 적합하지 않을 수 있다.

그럼 대체 어떻게 가려야 하는거야? 😫

우리의 이런 고민, 다행히도 우리나라의 대표적인 IT 기업들이 선행해서 고민해왔고, 각 기업마다 내부에서 사용하는 통일된 기법들이 존재한다.

이 글에서는 각 기업들의 어떤 방식으로 IR을 처리하는 지 살펴보도록 하자.

IR 기법
IR 기법은 이미지 대체텍스트 제공을 위한 CSS 기법으로 다양한 CSS 기법을 사용하여
이미지 대체텍스트를 제공할 수는 기법

1. Phark Method
   - 의미있는 이미지의 대체 텍스트를 제공하는 경우
   - 설정방법
     - 이미지로 대체할 엘리먼트에 배경이미지를 설정하고 글자는 text-indent를 이용하여 화면 바깥으로(-9999px 만큼 내어쓰기) 빼내어 보이지 않게 하는 방법

```
/* pc용 */
.ir_pm{
	display:block;
	overflow:hidden;
	Font-size:1px;
	line-height:0;
	text-indent:-9999px;
}

/* mobile용 */
.ir_pm{
	display:block;
	overflow:hidden;
	font-size:1px;
	line-height:0;
	color:transparent;
}
```

2. WA IR
   - 의미있는 이미지의 대체 텍스트로 이미지가 없어도 대체 텍스트를 보여주고자 할때
   - 중요한 이미지가 로딩이 되지 않거나, 손실되어 off될 때에도 대체 텍스트를 보여주고자 할 때 아래 .ir_wa 사용
   - 다른 기법과 다르게 width, height 를 100%를 주어 요소 그대로의 자리를 차지하고 있음.
   - 하지만 z-index 속성에 음수값을 주어 텍스트가 이미지 뒤에 숨도록 많듬
   - 이 기법은 img의 alt 값처럼 사용되는 백그라운드 판 alt라고 이해함
   - 설정방법
     - 이미지로 대체할 엘리먼트에 배경 이미지를 설정하고 글자는 span 태그로 감싼 후 z-index:-1 을 이용하여 화면에 안보이게 처리

```
.ir_wa {
  display: block;
  overflow: hidden;
  position: relative;
  z-index: -1;
  width: 100%;
  height: 100%;
}
```

```
<div class="header-icon">
  <a href="#" class="icon1"><span clas="ir_pm
</div>
```

스크린 리더가 읽을 필요 없는 경우
스크린 리더가 내용을 읽을 필요없고, 웹표준을 준수하기 위해 사용한 헤딩 등 마크업 구조상 필요한 경우가 존재하며
이경우, .screen_out 클래스를 사용함.
일반적인 스크린 리더들은 width와 height가 없는 요소들은 내용을 읽지 않음

```
.screen_out {
  overflow: hidden;
  position: absolute;
  width: 0;
  height: 0;
  line-height: 0;
  text-indent: -9999px;
}
```
