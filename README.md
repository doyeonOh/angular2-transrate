Angular2 번역
===================

Angular2>Basic>Angular Architecture
-------------

아키텍쳐 개요
--

angular2 는 HTML 와 자바스크립트를 사용하는 client application 을 빌드 할때 도움을 주는 프레임워크이다.

Framework 는 몇몇개의 협력적인 라이브러리로 구성되어 있으며, 그중 몇은 core 이고 몇은 option 이다.
우리는 angular 스러운 마크업으로 구성된 HTML template을 구성하고, 그 template 들을 관리하는 component 클래스는 작성하고, service에 application 로직을 추가하고 최상단 root component 인 Angular's bootstrapper 를 조작하여 application 을 작성한다.

Angular 는 브라우저에서 우리의 application content 를 보여주거나,  우리가 제공하는 안내에 따른 user interaction에  응답하는 것을 대신한다.  

 물론 지금 얘기한 것 보다는 더 많은 것이 있다. 우리는 guide chapter 를 들어갈 때 그 세부적인 내용을 배우게 될 것이다. 먼저 큰 그림을 그려보자.

![enter image description here](https://angular.io/resources/images/devguide/architecture/overview2.png)


이 아키텍쳐 diagram은 Angular2 application이 8개의 main building block 으로 되어 있다는 것을 표현한다.

1. 모듈
2. 컴포넌트
3. 템플릿
4. 메타데이타
5. 데이타 바인딩
6. 디렉티브
7. 서비스
8. 의존 주입

저 8개를 배우고 우리만의 방식으로 해보자.(??)

>The code referenced in this chapter is available as a live example.


모듈
--

 Angular 앱은 모듈 방식이다.
일반적으로 우리는 많은 모듈로 우리의 application 을 조립한다.
전형적인 모듈은 하나의 목적을 가진 응집력 있는 코드 블록이다.  모듈은 그 코드에서 일반적인 class 같은  무언가의 값을 export(내보내기)한다.


![enter image description here](https://angular.io/resources/images/devguide/architecture/module.png)

>모듈은 option 이다.
우리는 모듈 방식의 설계를 매우 추천한다. TypeScript 는 ES2015 모듈 문법을 많이 서포트하고 있고 챕터들은 그 문법들을 사용하게 함으로서 우리가 좀 더 모듈 방식에 가까워지도록 하게한다. 그것이 우리가 basic building blocks  사이에 모듈을 넣은 이유이다.

>Angular 는 스스로 모듈방식 접근이나 상세한 문법을 필요로 하지는 않는다. 만약 당신이 그 방식을 원하지 않으면 사용하지 않아도 된다. 각 챕터는 import 와 export 구문을 사용하는 깔끔한 방식을 충분히 제공한다.(?)

>모듈 시스템이 아니고 평범하고 오래된 자바스크립트를 사용하는 anagular2 개발을 보여주는 자바스크립트 트랙(이 페이지의 상단에 있는 콤보박스를 선택) 에서 설정 및 구성할 수 있는 방법을 찾을 수 있다.


 아마도 우리가 처음 만나게 될 첫 모듈은 compoent class 를 export 하는 모듈이다. Component 는 basic angular blocks 의 하나로서 우리는 그것을 많이 작성하게 될 것이고 다음 부분에서 component 들에 대하여 이야기 하게 될 것이다.  곧 Component class가 우리가 export 한 모듈의 종류의 하나인 것을 충분히 알게 될 것이다.

 대부분의 application 은 하나의 AppComponent 를 가지고 있다. 규칙에 의해 우리는 app.component.ts 라는 이름의 파일를 찾을 수 있다. 그 파일의 내용을 보면 우리는 다음과 같은 export 구문을 볼 수 있다.
```
app/app.component.ts(excerpt)
 export class AppComponent()
```
 export 구문은 AppComponent 클래스가 public 이고 application 의 다른 모듈에 접근할 수 있는 모듈인 것을 TypeScript 형식으로 말하고 있다.

 AppComponent 에 대한 참조가 필요할때 우리는 import 를 다음과 같이 할 수 있다.
```
app/main.ts (excerpt)
 import { AppComponent } from './app.component';
```
 import 구문은 이웃하고 있는 파일들 중 이름이 app.component 라는 모듈에서 AppComponent 를 가져오게 할 수 있도록 시스템에 말하고 있다. 모듈 이름( 모듈 id로 불려지는 )은 종종 확장자가 없는 filename 과 동일하다.

라이브러리 모듈들
---------


몇개의 모듈들은 다른 모듈의 라이브러리이다.
Angular 는 스스로 “barrels”라는 불리는 라이브러리 모듈들의 집합을 실어 나른다. 각각의 Angular 라이브러리는 실제적으로 public 한 외관 위에 몇몇개의 논리적으로 연결된 private 한 모듈들이 있다.
angular2/core 라이브러리는 우리가 가장 필요로하는 기본적인 angular 라이브러리 모듈이다.


또 다른 중요한 Angular 라이브러리로는 angular2/common, angular2/router 그리고 angular2/http 가 있다.

어떻게 Angular 모듈이 조직되고 분리되어 있는 지 더 배우고 싶다면 modules, barrels and bundles 를 참조


우리는 같은 방법으로 angular 라이브러리 모듈에서 우리가 원하는 것을 import 할 수 있다. 예를 들어서,  angular2/core 모듈을 사용하여 Angular Component 함수를 다음과 같이 import 할 수 있다.
```
import { Component } from 'angular2/core';
```
우리가 전에 import 한 AppComponent 의 문법과 비교해보자.
```
import { AppComponent } from './app.component';
```
다른 것을 알아 차렸는가? 첫번째는 Angular 라이브러리 모듈을 import 할 때, import 구문은 경로 접두어가 없는 빈 모듈 이름을  가져온다.

 우리가 우리의 파일에 있는 것을 import 할때는 파일의 경로가 포함된 모듈이름을 접두어로 붙인다. 이 예제에서 보면 상대경로(./)  를 기술하고 있다. 이것은 소스 모듈이 같은 폴더('./') 있는 모듈을 import 하는 것을 의미한다. application 폴더 구조에 대한 경로만 지정하면 소스 모듈이 어디에 있든 우리는 사용할 수 있다.(?)

우리는 ECMAScript 2015(ES2015) 모듈 문법으로 import 와 export 한다. 문법에 대해 더 알고 싶다면 여기 를 보고 다른 web 의 많은 장소에서도 볼 수 있다.

모듈 로딩이나 import 의  하부구조는 중요한 주제이다. 그러나 이 Angular 를 설명하는 범위 밖의 주제이다. import 와 export 는 우리 알아야 할 모든 것이니 우리는 application 에 좀 더 집중할 것이다..(?)


우리가 알아야 할 중요한 것은

- Angular App은 모듈들로 구성되어 있다.
- 모듈들은 다른 모듈 import 할 수 있는 class, function, value 들로 export 한다.
- 우리는 각 모듈이 한가지만 export 하는 모듈로 이루어진 application으로 작성하는 것을 선호한다.

우리가 작성하는 첫 번째 모듈은 대부분 component 를 export 한다.

컴포넌트
--
![enter image description here](https://angular.io/resources/images/devguide/architecture/hero-component.png)
컴포넌트는 우리가 View 를 호출 할 수 있는 화면 공간에 패치를 컨트롤 한다.(?)  네비게이션 링크를 가진 application root shell, list of heroes, hero editor 들은 모두 Component 에 의해 컨트롤되는 View 들이다.

우리는 클래스 안에 컴포넌트 application logic(view 를 서포트 한다) 을 정의한다. 클래스는 프로퍼티와 메소드 API를 통해서 View 와 상호작용을 한다.

예를 들어서 HeroListComponent 에는 service로 부터 얻은 heroes 배열을 돌려주는 heroes 라는 프로퍼티가 있을 수 있다. 또한 컴포넌트는 유저가 hero 리스트에서 한 hero 를 클릭할 때 selectedHero 라는 프로퍼티를 set 하는 selectHero() 라는 메소드도 가지고 있을지도 모른다. 그것을 class 로 표현하면 다음과 같을 것이다.

```
app/hero-list.component.ts

export class HeroListComponent implements OnInit {
	constructor(private _service: HeroService) { }

	heroes: Hero[];
	selectedHero: Hero;

	ngOnInit(){
		this.heroes = _service.getHeroes();
	}
	selectHero(hero:Hero) { this.selectedHero = hero; }
}
```
application 을 통해 유저가 움직일 때 마다 Angular 는 컴포넌트들을 생성하거나 수정하거나 소멸시킨다.  개발자는 선택적인 Lifecycle Hook 를 통해서 각 순간에 조치를 취할 수 있다.

>우리는 이 예제에서 저기있는 hook 들을 보여주진 않을 것이다. 그러나 저것들을 나중에 알 수 있도록 멘탈 노트를 만들 것이다.
>우리는 누가 저 생성자를 호출하는거야? 라고 궁금해 할지도 모른다. 누가 service 매개변수를 제공하는가? 잠시동안은 Angular 가 생성자를 호출하고 우리가 원할 때 적당한 HeroService 가져다주는 것만 알고 있으면 된다.
