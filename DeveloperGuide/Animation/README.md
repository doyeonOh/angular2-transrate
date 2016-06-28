Angular2 번역
===================

Angular2>developer guide>Animation
-------------

Animation
--

모션은 모던 웹 어플리케이션에서 디자인적으로 매우 중요한 부분이다. 우리는 User Interface 가 상태들 사이에서 smooth transition 을 갖고있고 그것을 필요로하는 곳에서 관심을 가지게하는 매력적인 animation 이기를 원한다. 잘 디자인된 animation 은 더 재미있을 뿐만 아니라 사용하기도 쉬운 UI 를 만들 수 있다.

Angular animation system 은 우리가 원하는 여러 animation 을 만들기 위해 필요한 것을 우리에게 제공한다. 우리는  순수 CSS animation 에 사용하는 native performance 와 같은 종류의 animation 을 빌드할 수 있다. 그러나 우리는 또한 우리의 animation logic 이 우리 application 의 나머지 부분과 밀접하게 통합되게 할 수 있다. 어디서든지 그것들은 쉽게 트리거되고 컨트롤 되어 진다.

>Angular animation 은 표준 Web Animation API 의 상단에 빌드되어 지고 그것을 지원하는 브라우저 위에서 native 하게 실행된다.
>다른 브라우저에서는 polyfill 이 필요하다. 여기에서 `web-animation.min.js` 를 잡고 당신의 페이지에 추가하라.
>Angular team 에 의해 유지되는 더 가벼운 polyfill 은 곧 나올 것입니다.

목차

- 퀵스타트 예제:  두 개의 상태에서의  트랜지셔닝
- 상태들과 트랜지션들
- 예제 : 진입과 이탈
- 예제 : 다른 상태로부터의 진입과 이탈
- 애니메이션 속성과 단위
- 자동 속성 계산
- 애니메이션 타이밍
- 키프레임을 사용한 다단계 애니메이션
- 평행한 애니메이션 그룹

>이 챕터의 예제 참조는 [live example](https://angular.io/resources/live-examples/animations/ts/plnkr.html) 에서 이용가능 합니다.
