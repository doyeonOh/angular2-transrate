Angular2 번역
===================

Angular2>Basic>Displaying Data
-------------

Displaying Data
--

우리는 일반적으로 Angular 에서 Angular Component 의 속성들을 HTML template 에 바인딩하여 data 를 표현한다.  

이번 챕터에서는 우리는 heroes 리스트를 가진 component 를 생성할 것이다. 각 hero 는 이름을 가지고 있다. 우리는 hero 이름들의 리스트를 표현할 것이고, 선택적으로 list 하단에 있는 상세 공간에서 선택된 hero 를 보여줄 것이다.

최종 UI 는 다음과 같이 보여진다.

![enter image description here](https://angular.io/resources/images/devguide/displaying-data/final.png)

차례

- 보간법( interpolation ) 과 함께 component 속성을 보여주기
- *ngFor 를 사용한 array 속성 보여주기
- *ngIf 를 사용한 선택적 표현하기

> [live example](https://angular.io/resources/live-examples/displaying-data/ts/plnkr.html) 는 이번 챕터의 모든 문법과 코드 예제를 모두 보여준다.

###보간법( interpolation ) 과 함께 component 속성을 보여주기

component 속성을 보여주는 제일 빠른 방법은 보간법 (interpolation) 을 통해 속성 이름을 바인딩 하는 것이다. 보간법에서는, 우리는 view template 안에 두개의 중괄호 사이에 속성 이름을 합쳐서 넣는다. `{{ myHero }}`

설명할 수 있는 작은 예제를 함께 빌드해보자.

새로운 프로젝트 폴더 (displaying-data) 를 생성하고 [QuickStart](https://angular.io/docs/ts/latest/quickstart.html) 에 있는 스텝들을 따라해보자.

> 대안으로, [QuickStart source 를 다운로드](https://github.com/angular/quickstart/blob/master/README.md)해서 시작할 수 있다.

그러면 template 과 component 몸체를 변경하여`app.component.ts`파일을 수정하자. 완료했다면, 다음과 같이 보일 것이다.

```
app/app.component.ts

import { Component } from '@angular/core';

@Component({
	selector: 'my-app',
	template: `
		<h1>{{title}}</h1>
		<h2>My favorite hero is:  {{myHero}}</h2>
	`
})
export class AppComponent {
	title = 'Tour of Heroes';
	myHero = 'Windstorm';
}
```

우리는 일반적인 비어있는 component 안에 `title` 과 `myHero` 라는 2개의 속성을 추가했다.   
수정된 template 은 이중 중괄호 보간법을 사용한 2개의 component  속성을 표현한다.

```
template : `
	<h1>title</h1>
	<h2>My favorite hero is: {{myHero}}</h2>
`
```
>template 은 ECMAScript 2015의 역따옴표(`) 안에 multi line string 으로 표현한다. 따옴표와 다른 성격인 역따옴표는 많고 좋은 형태들을 가지고 있다. 우리가 여기서 이용한 특징은 여러개의 line 들을 결합하는 능력이다.  이것은 가독성이 좋은 HTML 을 만들어준다.

Angular 는 자동으로 component 로부터 `title` 과 `myHero` 속성값을 가져온다. 그리고 browser 안에 그 값들을 삽입한다. 그리고 Angular 는 그 값들이 변할 때 수정된 것을 보여준다.

>더 정확하게, 다시 보여지는 화면은 keystroke, timer completion 또는 비동기 `XHR` 응답과 같은 view 에 연결되어진 몇개의 비동기적 이벤트 후에 발생한다. 이번 sample 에 저런 것들을 가지고 있진 않지만 그 속성들은 그것들 스스로 변경되진 않는다. 순간에 작동한다는 믿음을 가져야 한다.

우리가 `AppComponent` 의 instance 를 생성하는 **new** 연산자를 부르지 않았다는 것을 알아차리자. Angular 는 우리를 위해 instance 를 어떻게 생성하는가?

"my-app" 이라는 요소 이름을 지정하는 `@Component`decorator 안의 CSS `selector`을 보자. QuickStart 의 기억을 되돌리면, 우리는 `index.html` 의 몸체에 `<my-app>` 이라는 요소를 추가했었다.

```
<body>
	<my-app>loading ... </my-app>
</body>
```

`AppComponent` 클래스(main.ts 에서 보면) 와 함께 bootstrap 할때, Angular 는 `index.html` 안에 `<my-app>`을 찾는다.  그것을 찾으면, `AppComponent` 의 instance 를 인스턴스화 한다. 그리고 `<my-app` 태그 안에 그것을 렌더링한다.

우리는 변경사항을 보는동안 우리 application 에 컴파일과 서비스하는 모든 npm script 가 실행되었기 때문에 동작하는 app 에서 변경되는 것을 볼 준비가 되었다.

```
npm start
```

우리는 title 과 hero 이름을 볼 수 있게 된다.

![enter image description here](https://angular.io/resources/images/devguide/displaying-data/title-and-hero.png)

우리가 대안을 만들고 고려한 몇개의 선택지를 review 해보자.

###템플릿 인라인 또는 템플릿 파일?

우리는 두군데 장소 중 하나에 component template 을 저장할 수 있다. `template` 속성을 사용해서 그것을 *inline* 으로 정의할 수 있다.  또는 우리는 별도의 HTML 안에서 template 을 정의 할 수 있고, `@Component` decorator 의 `templateUrl` 속성을 이용하여 component metadata 에서 그것들을 연결 할 수 있다.

inline 형식과 별도의 HTML 사이에서의 선택은 취향, 상황, 조직 정책의 문제이다. 여기서 우리는 inline 형식을 사용한다. 왜냐하면 template 이 작고 demo 가 HTML file 이 없어도 간단하기 때문이다.

어떤 스타일이든, template data binding 이 component 속성에 동일한 접근을 한다.

### 생성자 또는 변수 초기화?

우리는 변수 할당을 사용해서 component 속성들을 초기화했다. 이것은 매우 간결하고 완벽한 테크닉이다.

몇몇의 사람들은 다음과 같이 생성자 안에서 속성을 선언하거나 초기화하는 것을 선호한다.

```
export class AppCtorComponent {
	title: string;
	myHero: string;

	constructor(){
		this.title = 'Tour of Heroes';
		this.myHero = 'Windstorm';
	}
}
```
그것도 역시 괜찮은 방법이다. 선택은 취향이나 조직 정책의 문제이다. 우리는 간단하게 이번 챕터에서 더욱 간결한 "변수 할당" 스타일을 채택할 것이다. 왜냐하면 그것이 읽는데 적은 코드가 들기 때문이다.

### *ngFor 를 사용한 array 속성 보여주기

우리는 hero 들의 list 를 보여주길 원한다. 우리는 component 에 모조의 hero 들의 이름을 가진 array 를 `myHero` 위에 추가하는 것으로 시작한다.  그리고 array 의 첫번째 이름을 `myHero` 에 다시 정의 한다.

```
app/app.component.ts (class)

export class AppComponent {
	title = 'Tour of Heroes';
	heroes = ['Windstorm', 'Bombasto', 'Magneta', 'Tornado'];
	myHero = this.heroes[0];
}
```

이제 우리는 `heroes` 리스트 안에 각각의 item 들을 template 에 표현하기 위해 Angular 의 `ngFor` "반복자" directive 를 사용한다.

```
app/app.component.ts (template)

template: `
	<h1>{{title}}</h1>
		<h2>My favorite hero is: {{myHero}}</h2>
		<p>Heroes:</p>
		<ul>
			<li *ngFor="let hero of heroes">
				{{ hero }}
			</li>
		</ul>
`
```
우리의 표현은  친숙한 HTML 인`<ul>` 과 `<li>` 태그를 사용한 정렬되지 않은 리스트이다. 자 이제 `<li>` 태그에 집중해보자.

```
<li *ngFor="let hero of heroes">
	{{ hero }}
</li>
```

우리는 뭔가 미스테리한 `*ngFor` 와 `<li>` 태그를 추가했다. 그것은 Angular 의 "반복자" directive 이다. `<li>` 태그에 마크된 그것은 존재는 `<li>` 요소 ( 그리고 그것의 자식들도) 가 "반복자 template" 이라는 것을 말한다.

> `*ngFor` 의 앞에 있는 별표(*) 를 잊으면 안된다. 그것은 문법에서 필요한 부분이다. 이것과 관련한 것과 `ngFor`에 대해서 더 알고 싶다면 [Template Syntax](https://angular.io/docs/ts/latest/guide/template-syntax.html#ngFor) 챕터를 참조해라.

`ngFor` 쌍따옴표 지시자 안에 `hero` 를 보자. 이것은 [template input variable](https://angular.io/docs/ts/latest/guide/template-syntax.html#ngForMicrosyntax) 의 하나의 예이다.

Angular 는 list 안에 각각의 item 을 위해서 `<li>` 를 반복하고, 현재의 iteration 안에 있는 item 에 hero 변수들을 세팅한다. Angular 는 이중 중괄호 보간법을 위한 컨텍스트로 그 변수들을 사용한다.

>우리는 배열을 표시하려면 `ngFor`를 주어야 한다. 사실, `ngFor` 는 어떠한 [iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) 객체의 item 도 반복할 수 있다.

여전히 `npm start` 명령어 아래 동작되고 있다면, 우리는 정렬되지 않은 리스트 안에 heroes 가 표시된 것을 볼 수 있다.

![enter image description here](https://angular.io/resources/images/devguide/displaying-data/hero-names-list.png)


### 데이터를 위한 클래스 생성하기

우리는 component 에 직접적으로 데이터를 정의했다. 그것은 데모에서는 괜찮은 방법이지만 확실히 최선의 방법은 아니다. 그리고 좋은 방법도 아니다. 비록 우리는 이번 챕터에서 그것에 대해 아무것도 하지 않지만, 우리는 그런 부분들을 해결할 수 있는 멘탈노트를 만들 것이다.

지금, 우리는 string array 에 바인딩 하고 있다. 우리는 때때로 실제 application 에서 그것을 행한다. 그러나 대부분 시간에서 우리는 객체(잠재적으로는 클래스의 instance)를 표현한다.

이제 `Hero` 객체 배열이 우리의 hero 이름 배열로 되게 해보자. 일단 우리는 `Hero` class 가 필요하다.

`app/` 폴더 안에  다음과 같은 약간의 코드를 가진 `hero.ts` 라고 불리는 새로운 파일을 만든다.

```
app/hero.ts

export class Hero {
	constructor(
		public id: number,
		public name: string
	) { }
}
```

우리는 생성자와 2개의 속성(id, name)을 가진 클래스를 정의했다.

그것은 우리가 속성을 가진것 처럼 보이지 않을 수도 있다. 그러나 우리는 가지고 있는 것이다. 우리는 생성자 파라메터의 선언안에서 TypeScript 의 바로가기(shortcut)를 활용할 수 있다.

첫번째 파라메터에 대해서 생각해보자.

```
public id: number,
```

이 간단한 문법은 많은 작업을 수행한다.

- 생성자 파라메터와 그것의 타입을 선언한다.
- 같은 이름의 public 한 속성을 선언한다.
- 우리가 클래스 인스턴스를 새로 만들 때, 해당 파라메터의 속성을 초기화 한다.

### Hero 클래스 사용하기

우리 component 안에 hero 객체의 배열을 돌리기 위한  `heroes` 속성을 다시 정의하고, 또한 mock Heroes의 첫번째를 `myHero` 속성에 세팅하자.

```
app.component.ts (excerpt)

heroes = [
	new Hero(1, 'Windstorm'),
	new Hero(13, 'Bombasto'),
	new Hero(15, 'Magneta'),
	new Hero(20, 'Tornado')
];

myHero = this.heroes[0];
```

우리는 template 을 업데이트 해야한다. 순간 그것은 완전한 `hero` 객체를 표현한다. (그것은 string value 로 사용된다). `hero.name` 속성	을 삽입하면서 그것을 수정해보자.

```
app.component.ts (template)

template: `
	<h1>{{title}}}</h1>
	<h2>My favorite hero is: {{myHero.name}} </h2>
	<p>Heroes: </p>
	<ul>
		<li *ngFor="let hero of heroes">
			{{ hero.name }}
		</li>
	</ul>

`
```

display 는 보기에 전과 똑같다. 그러나 지금 우리는 hero 가 진짜 무엇인지 좀 더 알게 되었다.
