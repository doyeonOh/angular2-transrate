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

>The code referenced in this chapter is available as a live [example](https://angular.io/resources/live-examples/architecture/ts/plnkr.html).


모듈
--

 Angular 앱은 모듈 방식이다.
일반적으로 우리는 많은 **모듈**로 우리의 application 을 조립한다.
전형적인 모듈은 하나의 목적을 가진 응집력 있는 코드 블록이다.  모듈은 그 코드에서 일반적인 class 같은  무언가의 값을 **export**(내보내기)한다.


![enter image description here](https://angular.io/resources/images/devguide/architecture/module.png)

>**모듈은 option 이다.**
우리는 모듈 방식의 설계를 매우 추천한다. TypeScript 는 ES2015 모듈 문법을 많이 서포트하고 있고 챕터들은 그 문법들을 사용하게 함으로서 우리가 좀 더 모듈 방식에 가까워지도록 하게한다. 그것이 우리가 basic building blocks  사이에 모듈을 넣은 이유이다.

>Angular 는 스스로 모듈방식 접근이나 상세한 문법을 필요로 하지는 않는다. 만약 당신이 그 방식을 원하지 않으면 사용하지 않아도 된다. 각 챕터는 `import` 와 `export` 구문을 사용하는 깔끔한 방식을 충분히 제공한다.(?)

>모듈 시스템이 아니고 평범하고 오래된 자바스크립트를 사용하는 anagular2 개발을 보여주는 자바스크립트 트랙(이 페이지의 상단에 있는 콤보박스를 선택) 에서 설정 및 구성할 수 있는 방법을 찾을 수 있다.


 아마도 우리가 처음 만나게 될 첫 모듈은 compoent class 를 export 하는 모듈이다. Component 는 basic angular blocks 의 하나로서 우리는 그것을 많이 작성하게 될 것이고 다음 부분에서 component 들에 대하여 이야기 하게 될 것이다.  곧 Component class가 우리가 export 한 모듈의 종류의 하나인 것을 충분히 알게 될 것이다.

 대부분의 application 은 하나의 `AppComponent` 를 가지고 있다. 규칙에 의해 우리는 `app.component.ts` 라는 이름의 파일를 찾을 수 있다. 그 파일의 내용을 보면 우리는 다음과 같은 `export` 구문을 볼 수 있다.
```
app/app.component.ts(excerpt)
 export class AppComponent()
```
 `export` 구문은 `AppComponent` 클래스가 public 이고 application 의 다른 모듈에 접근할 수 있는 모듈인 것을 TypeScript 형식으로 말하고 있다.

 `AppComponent` 에 대한 참조가 필요할때 우리는 **import** 를 다음과 같이 할 수 있다.
```
app/main.ts (excerpt)
 import { AppComponent } from './app.component';
```
 `import` 구문은 이웃하고 있는 파일들 중 이름이 `app.component` 라는 모듈에서 `AppComponent` 를 가져오게 할 수 있도록 시스템에 말하고 있다. **모듈 이름**( 모듈 id로 불려지는 )은 종종 확장자가 없는 filename 과 동일하다.

**라이브러리 모듈들**

몇개의 모듈들은 다른 모듈의 라이브러리이다.
Angular 는 스스로 “barrels”라는 불리는 라이브러리 모듈들의 집합을 실어 나른다. 각각의 Angular 라이브러리는 실제적으로 public 한 외관 위에 몇몇개의 논리적으로 연결된 private 한 모듈들이 있다.
`angular2/core` 라이브러리는 우리가 가장 필요로하는 기본적인 angular 라이브러리 모듈이다.


또 다른 중요한 Angular 라이브러리로는 `angular2/common,` `angular2/router` 그리고 `angular2/http` 가 있다.

>어떻게 Angular 모듈이 조직되고 분리되어 있는 지 더 배우고 싶다면 [modules, barrels and bundles](https://github.com/angular/angular/blob/master/modules/angular2/docs/bundles/overview.md) 를 참조


우리는 같은 방법으로 angular 라이브러리 모듈에서 우리가 원하는 것을 import 할 수 있다. 예를 들어서,  *angular2/core* 모듈을 사용하여 Angular `Component` **함수**를 다음과 같이 import 할 수 있다.
```
import { Component } from 'angular2/core';
```
우리가 전에 import 한 AppComponent 의 문법과 비교해보자.
```
import { AppComponent } from './app.component';
```
다른 것을 알아 차렸는가? 첫번째는 Angular 라이브러리 모듈을 import 할 때, import 구문은 경로 접두어가 없는 빈 모듈 이름을  가져온다.

 우리가 우리의 파일에 있는 것을 import 할때는 파일의 경로가 포함된 모듈이름을 접두어로 붙인다. 이 예제에서 보면 상대경로(./)  를 기술하고 있다. 이것은 소스 모듈이 같은 폴더('./') 있는 모듈을 import 하는 것을 의미한다. application 폴더 구조에 대한 경로만 지정하면 소스 모듈이 어디에 있든 우리는 사용할 수 있다.(?)

>우리는 ECMAScript 2015(ES2015) 모듈 문법으로 import 와 export 한다. 문법에 대해 더 알고 싶다면 [여기](http://www.2ality.com/2014/09/es6-modules-final.html) 를 보고 다른 web 의 많은 장소에서도 볼 수 있다.

>모듈 로딩이나 import 의  하부구조는 중요한 주제이다. 그러나 이 Angular 를 설명하는 범위 밖의 주제이다. import 와 export 는 우리 알아야 할 모든 것이니 우리는 application 에 좀 더 집중할 것이다..(?)


우리가 알아야 할 중요한 것은

- Angular App은 모듈들로 구성되어 있다.
- 모듈들은 다른 모듈 import 할 수 있는 class, function, value 들로 export 한다.
- 우리는 각 모듈이 한가지만 export 하는 모듈로 이루어진 application으로 작성하는 것을 선호한다.

우리가 작성하는 첫 번째 모듈은 대부분 component 를 export 한다.

컴포넌트
--
![enter image description here](https://angular.io/resources/images/devguide/architecture/hero-component.png)
**컴포넌트**는 우리가 View 를 호출 할 수 있는 화면 공간에 패치를 컨트롤 한다.(?)  네비게이션 링크를 가진 application root shell, list of heroes, hero editor 들은 모두 Component 에 의해 컨트롤되는 View 들이다.

우리는 클래스 안에 컴포넌트 application logic(view 를 서포트 한다) 을 정의한다. 클래스는 프로퍼티와 메소드 API를 통해서 View 와 상호작용을 한다.

예를 들어서 `HeroListComponent` 에는 service로 부터 얻은 heroes 배열을 돌려주는 `heroes` 라는 프로퍼티가 있을 수 있다. 또한 컴포넌트는 유저가 hero 리스트에서 한 hero 를 클릭할 때 `selectedHero` 라는 프로퍼티를 set 하는 `selectHero()` 라는 메소드도 가지고 있을지도 모른다. 그것을 class 로 표현하면 다음과 같을 것이다.

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
application 을 통해 유저가 움직일 때 마다 Angular 는 컴포넌트들을 생성하거나 수정하거나 소멸시킨다.  개발자는 선택적인 [Lifecycle Hook](https://angular.io/docs/ts/latest/guide/lifecycle-hooks.html) 를 통해서 각 순간에 조치를 취할 수 있다.

>우리는 이 예제에서 저기있는 hook 들을 보여주진 않을 것이다. 그러나 저것들을 나중에 알 수 있도록 멘탈 노트를 만들 것이다.
>우리는 누가 저 생성자를 호출하는거야? 라고 궁금해 할지도 모른다. 누가 service 매개변수를 제공하는가? 잠시동안은 Angular 가 생성자를 호출하고 우리가 원할 때 적당한 `HeroService` 가져다주는 것만 알고 있으면 된다.

템플릿
--
![enter image description here](https://angular.io/resources/images/devguide/architecture/template.png)
우리는 컴포넌트 view 와 그의 짝인 **template** 을 함께 정의한다.(?) template은 HTML 으로 이루어진 폼으로 Component 를 어떻게 렌더링 하는지 Angular 에 말하는 역할을 한다.
템플릿은 일반 HTML 과 많은 부분에 있어서 비슷하다. 그리고 좀 이상한 부분을 갖는다. 여기에 템플릿인 우리의 `HeroList` component 가 있다.

```
app/hero-list.component.html

<h2>Hero List</h2>

<p><i>Pick a hero from the list </i></p>
<div *ngFor="let hero o heroes" (click)="selectHero(hero)">
	{{hero.name}}
</div>

<hero-detail *ngIf="selectedHero" [hero]="selectedHero"></hero-detail>
```
우리는 `<h2>` 와 `<div>` 태그 를 알아본다. 그러나 다른 마크업들은 학교에서 어느 누구도 알려주지 않았다.
`*ngFor`, `{{ hero.name }}`, `(click)`, `[hero]`, 그리고 `<hero-detail>` 은 무엇이란 말인가?

저기에 Angular 의 [템플릿 문법](https://angular.io/docs/ts/latest/guide/template-syntax.html)의 예제들이 있다. 우리는 그 문법에 익숙해지도록 성장할 것이고, 심지어 그 문법을 배우는 것을 사랑할지도 모른다. 우리는 곧 설명을 시작할 것이다.

진행하기 전에, 마지막 라인에 집중 해주기 바란다. `<hero-detail>` 태그는 `HeroDetailComponent` 를 나타내는 커스텀 엘리먼트이다.
우리가 봤을 때 `HeroDetailComponent` 는 `HeroListComponent` 와는 다른 Component 이다. `HeroDetailComponent` (코드를 보여주지 않았다) 는 특정한 Hero 를 표현하고,  유저가 리스트로부터 선택당하는 Hero 는 `HeroListComponent` 로부터 표현한다.   즉, `HeroDetailComponent` 는 `HeroListComponent` 의 자식이다.

![enter image description here](https://angular.io/resources/images/devguide/architecture/component-tree.png)

우리가 이미 알고 있는 HTML 엘리먼트들 사이에서 `<hero-detail>` 이 어떻게 편안히 자리잡고 있는지 관심을 가져야 한다. 우리는 같은 레이아웃에 있는 native HTML 과 함께 우리의 커스텀 엘리먼트를 합칠 수 있다.
그리고 이런 방법으로 우리는 rich featured application를 구축하기 위한 복잡한 Component 트리들을 구성 할 수  있다.

Angular 메타데이터
--

![enter image description here](https://angular.io/resources/images/devguide/architecture/metadata.png) 메타데이터는 Angular가 클래스로 어떻게 처리되는지 알려준다.
`HeroListComponent` 를 다시 보자. 그냥 그런 class 로 보인다. framework 라는 단서가 없다. 모든 부분에 Angular 가 존재하지 않는다.  
사실, 그건 진짜 *그냥 class* 이다. 우리가 Angular 에게 말하기 전까지는 component 가 아니다.
우리는 `HeroListComponent` 가  class 에 **metadata** 가 첨부된 component 라는 것을 Angular 에게 알려준다.
Typescript 에서 metadata 를 첨부하는 쉬운 방법은 decorator 와 함께 하는 것이다.
이것은 `HeroListComponent` 의 몇개의 메타데이터이다.

```
app/hero-list.component.ts (metadata)

@Component({
	selector: 'hero-list',
	templateUrl: 'app/hero-list.component.html',
	directives: [HeroDetailComponent],
	providers: [HeroService]
})
export class HeroListComponent{ ... }
```

여기에서 우리는 Component class 의 바로 아래에 있는 class 를 식별하는 `@Component` decorator 를 볼 수 있다.  `@Component` decorator 는 Angular 가 Component 와 그것의 View 를 생성하거나 표현하는 것에 대한 필요 정보를 포함하고 있는 필수 설정 Object를 가져야 한다.(?)

 여기에서 몇개의  `@Component` 설정 옵션을 볼 수 있다.


 - `selector` - Angular 가 부모 HTML 안에 있는 `<hero-list>` 태그를 찾은 곳에서 component 의 instance 를 생성하거나 삽입 할 수 있도록 알려주는 css selector 이다. application shell(component) 에 template 이 포함되어 있다면, Angular 는 `HeroListComponent` view 의 instance 를 저 태그들 사이에 삽입한다.  
```
<hero-list></hero-list>
```
 - `templateUrl` - 위에서 보여준 component template의 주소이다.   

 - `directives` - 이 template 이 필요로 하는 Component 들이나 Directive 들의 배열이다. 우리는 `<hero-detail>` 태그로 표시된 공간에  Angular 가 `HeroDetailComponent` 를 삽입 할 것이라고 예상했던  template 의 마지막 라인을 보았었다. 오직 우리가 `HeroDetailComponent` 를  여기 있는 `directivces` 배열에 언급을 했을 때에만 Angular 는 그렇게 할 것이다.
 - `providers` - component 가 필요로하는 service에 대한 dependency injection providers(의존주입제공자) 의 배열이다. 이것은 우리의 component 생성자가 `HeroService` 를 필요하다는 것을 Angular 에게 알려주는 한가지 방법이다. 그리고 그것을 사용하여 화면에 그릴 수 있는 영웅들의 리스트를 얻을 수 있다. 우리는 순식간에 dependency injection(의존주입) 을 얻을 수 있다.  

![enter image description here](https://angular.io/resources/images/devguide/architecture/template-metadata-component.png)  

`@Component` 함수는 설정 object 를 가지고 있고 이 component class 정의와 연결된 metadata 로 변한다. Angular 는 runtime 때 이 metadata 를 발견하고 따라서 "옳은 일(the right thing)" 하는 방법을 알고 있다.

template, metadata, 그리고 component 는 view 를 함께 그린다.

우리는 Angular 동작을 안내한 것과 비슷한 방식으로 다른 metadata decorator 를 적용할 수 있다. `@Injectable`, `@Input`, `@Output`, `@RouterConfig` 는 우리가 Angular 지식이 성장함에 따라 마스터해야 될  좀 더 자주 쓰이는 decorator 들이다.  

구조적인 take-away 란 우리의 코드에 metadata 를 반드시 추가하는 것이다. 그래야 Angular 가 우리가 무엇을 하는지 알게된다.

데이타 바인딩
--
프레임워크 없이 우리는 HTML 컨트롤로 데이터 값들을 push 하고 동작과 값 업데이트로 유저 응답을 돌려주는 것을 책임져야한다.  Writing such push/pull logic by hand is tedious, error-prone and a nightmare to read as the experienced jQuery programmer can attest.(해석 불가)

![enter image description here](https://angular.io/resources/images/devguide/architecture/databinding.png)

Angular 는 **데이타 바인딩**을 지원한다. template 의 부분들과 component 의 부분들을 조정하는 메카니즘이다.  template HTML 에 바인딩 마크업을 추가함으로써 양측에 어떻게 연결할 수 있는지 Angular 에 알려준다.
데이터 바인딩 문법의 4가지 형식이 있다. 각 형식은 위 다이어그램 안에 화살표로 가리켜진 것 처럼 방향 (dom에서, dom으로 부터, 또는 양방향 ) 을 가지고 있다.

우리는 우리의 예제 template 에서 3가지의 데이터바인딩을 보았다.

```
app/hello-list.component.html(excerpt)

<div>{{hero.name}}</div>
<hero-detail [hero]="selectedHero"></hero-detail>
<div (click)="selected(hero)"></div>
```

- "interpolation"( {{ }} 의미하는 듯)은 `<div>` 태그안에 있는 component 의 `hero.name`의 속성을 표시한다.
- `[hero]` 속성 바인딩은 부모인 `HeroListComponent`로 부터 온 `selectedHero` 를 자식인 `HeroDetailComponent` 에 `hero` 속성으로 전달한다.
- `(click)` 이벤트 바인딩은 영웅의 이름에 유저가 클릭 했을 때 component 의 `selectHero` 메소드를 호출한다.

**양방향 데이터 바인딩**은 `ngModel` directive 를 사용한 단일 표기 안에서 속성과 이벤트 바인딩이 합쳐진 중요한 4번째 형식이다. 우리는 `HeroListComponent` 안에서 양방향 데이터 바인딩을 갖고 있지 않았다. 여기에 `HeroDetailComponent` template 으로부터 온 예제가 있다.(보여주지않았음)

```
<input [(ngModel)]="hero.name">
```
앙뱡향 데이터 바인딩에 있어서, 데이터 속성값은 속성 바인딩이 되어있는 Component 에서 input box 로 흐른다. 이벤트 바인딩처럼 유저에 의한 변경(최신값으로 속성을 재설정)은 또한 component 로 되돌아 흐른다.

Angular는 application component tree 의 루트로부터 깊이우선인 Javascript Event Cycle 한번 당 *모든* 데이터바인딩을 처리한다.(?)

우리는 아직 모든 것에 대한 세부적인 사항은 알 수 없다. 그러나 데이터 바인딩은 template 과 그것의 component 의 통신안에서 중요한 역할을 한다는 것은 예제를 통해 분명해졌다.

![enter image description here](https://angular.io/resources/images/devguide/architecture/component-databinding.png)

... **그리고** 부모와 자식 component 사이에서도


디렉티브
--

![enter image description here](https://angular.io/resources/images/devguide/architecture/directive.png)
우리의 template 들은 *동적*이다. Angular 가 그것들을 렌더할 때, 그것은 주어진 directive 의 지시에 따라서 DOM 을 변환한다.
directive 는 directive metadata 를 가지고 있는 하나의 클래스이다. TypeScript 에서, 우리는 클래스에 metadata 에 첨부할 `@Directive` decorator 를 적용할 것이다.
우리는 이미 directive 의 한가지 형식(component)을 만났었다. component 는 *template 을 가지고 있는 directive* 라고 하고 실제로 `@Component` decorator 는 template 중심의 기능을 확장한 `@Directive` decorator 이다.

>component 는 기술적으로 directive 이지만, 그것은 매우 고유하면서 우리의 아키텍쳐 개요에 있어 directive 부터 component 를 분리하기 위해 선택하는 Angular application 의 중심이다.

두 개의 *다른* directive 가 있으며, "구조" 와 "속성" directive 라고 불린다.
그것들은 가끔은 이름 자주로는 할당 또는 바인딩의 대상인 속성과 같은 엘리먼트 태그를 포함하여 나타나는 경향이 있다.
**구조** directive 는 추가, 삭제 그리고 DOM 에서 엘리먼트 대체에 의해 layout 을 바꾼다.
우리는 두 개의 내장된 구조 directive 를 사용하는 다음 예제 template 을 통해 볼 수 있다.

```
<div *ngFor="let hero of heroes"></div>
<hero-detail *ngIf="selectedHero"></hero-detail>
```
- `*ngFor` 는 `heroes` 리스트의 hero 당 하나의 `<div>` 를 찍어내도록(stamp out) Angular 에게 말한다.
- `*ngIf` 는 오직 선택한 hero 가 존재할 때, `HeroDetail` component 를 포함하도록 한다.

**속성** directive 는 존재하는 엘리먼트의 모양이나 행동을 바꾼다. template 에서 이름 때문에 그것들은 일반 HTML 처럼 보인다,
양방향 데이터 바인딩을 구현한 `ngModel` directive 은 속성 directive 의 예시이다.

```
<input [(ndModel)]="hero.name">
```

그것은 보여지는 값 속성을 세팅하거나 변경 이벤트에 응답함으로써 존재하는 엘리먼트(위에서는 `<input>`) 의 행동을 수정한다.
Angular 는 layout 구조를 바꾸거나(예를 들어 ngSwitch) DOM 엘리먼트와 component 의 부분을 수정하거나(예를 들어 ngStyle 그리고 ngClass) 하는 몇가지 다른 directive 를 제공한다.
그리고 당연하게도 우리는 우리만의 directive 들을 작성 할 수 있다.

서비스
--

![enter image description here](https://angular.io/resources/images/devguide/architecture/service.png)
"서비스"는 값이나 우리 application 에 필요한 기능 혹은 특징을 아우르는 광범위한 범주이다.  
거의 무엇이든 service 가 될 수 있다. service 는 일반적으로 제한되고 잘 정의된 목적을 가진 클래스이다. service 는 무엇인가를 특정해야하고 또한 그것이 잘 동작하게 해야 한다.

예는 다음과 같다.

- 로깅 서비스
- 데이터 서비스
- 메시지 버스
- 세금 계산
- application 설정

구체적인 Angular 에 대한 서비스는 없다. Angular 는 스스로 서비스의 정의는 가지고 있지 않다. ServiceBase 클래스는 없다. 그러나 service 는 어떠한 Angular application 에 있어 기본이다.
여기에 browser console 에 log 를 남기는 service class 의 예제가 있다.

```
app/logger.service.ts(class only)

export class Logger{
	log(msg: any)   { console.log(msg); }
	error(msg: any) { console.error(msg); }
	warn(msg: any)  { console.warn(msg); }
}
```

여기에 heroes 를 가져오고 resolve 된 promise 에서 그들을 반환하는 `HeroService` 가 있다. `HeroService` 는 `LoggerService` 와 서버 커뮤니케이션 그런트 작업(the server communication grunt work)을 조작하는 또 다른 `BackendService` 에 의존한다.

```
app/hero.service.ts(class only)

export class HeroService{
	constructor(
		private backend : BackendService,
		private logger : Logger
	) { }

	private heroes: Hero[] = [];

	getHeroes() {
		this.backend.getAll(Hero).then( (heroes: Hero[]) => {
			this.logger.log(`Fetched ${heroes.length} heroes.`);
			this.heroes.push(...heroes); // fill cache
		});
		return this.heroes;
	}
}
```

service 는 어디에든지 있다.
우리의 component 는 service 의 큰 소비자이다. 그들은 대부분의 하기싫은일을 핸들링하는 service 에 의존적이다. 그들은 서버로부터 data 를 가져오지 않고, 유저 입력에 대한 유효성검증도 하지 않고, console 에 직접적으로 log 도 남기지 않는다. 그들은 그와같은 일들을 service 에 위임한다.

component 의 일은 유저 경험(user experience)을 가능하게 하는 것 그 이상 그 이하도 아니다. 그것은 view(template 에 의해 렌더링된) 와 application logic(이것은 종종 몇몇의 "모델" 개념을 포함한다) 사이를 집중한다. 좋은 component 는 데이터바인딩을 위한 속성과 함수들을 제공한다. 그것은 모든 중요한 것들을 service 에 위임한다.

Angular 는 그들의 원칙을 *강요*하지는 않는다. Angular 는  우리가 3000라인의 "흔한(kitchen sink)" 컴포넌트를 작성하더라도 불평하지 않는다.

Angular 는 쉽게 우리 application logic 을 내부의 있는 service 에  반영할 수 있게 만들어주거나  *의존주입*을 통해 component 가 service 를 이용 가능하도록 만들어주게 함으로써 그런 원칙들을 따를 수 있게 도움을 준다.
