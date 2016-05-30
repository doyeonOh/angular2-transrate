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
- NgFor 를 사용한 array 속성 보여주기
- NgIf 를 사용한 선택적 표현하기

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
