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

퀵스타트 예제:  두 가지 상태에서의 트랜지셔닝
-

모델 속성에 의해 두가지 상태를 트랜지셔닝하는 심플한 애니메이션을 빌드해보자.

Animation 들은 내부에 `@Component` 메타데이터를 정의한다. 우리가 몇몇을 추가하기 전에, 우리는 약간의 animation specific function 을 가져와야 한다.

```
import {
	Component,
	Input,
	trigger,
	state,
	style,
	transition,
	animate
} from '@angular/core';
```

우리는 이제 component metadata 에서 `heroState` 라고 불리는 *animation trigger* 를 정의할 수 있다.  그것은 두 가지 상태(active 와 inactive)에서 트랜지셔닝하는 것을 animate 한다. hero 가 active 할 때, 우리는 약간 큰 사이즈와 밝은 색상으로 element 를 표현한다.

```
animations: [
	trigger('heroState', [
		state('inactive', style({
			background-color: '#eee',
			transform: 'scale(1)'
		})),
		state('active', style({
			background-color: '#cfd8dc',
			transform: 'scale(1.1)'
		})),
		transition('inactive => active', animate('100ms  ease-in')),
		transition('active => inactive', animate('100ms ease-out'))
	])
]
```

>이 예제에서는 우리가 animation metadata 에 직접 inline 으로 animation style (color and transform) 을 정의했다. 곧 다가올 Angular 릴리즈는 Component CSS Stylesheet 로부터 가져오는 것을 추가 할 수 있도록 지원 할 것이다.

자 이제 우리는 정의된 Animation 을 가지고 있다. 하지만 그것은 아직 어느 곳에서도 사용하지 못한다. 우리는 `@triggerName` 문법을 사용하여 component 의 template 에 하나 이상의 element 에 부착하여 이를 변경 할 수 있다.

```
template: `
	<ul>
		<li *ngFor="let hero of heroes"
			@heroState="hero.state"
			(click)="hero.toggleHero()">
			{{ hero.name }}
		</li>
	</ul>

`
```
여기에 우리는 `ngFor`에 의해 모든 element 들이 반복되어지는 animation trigger 를 적용했다. 각각의 반복되어지는 element 들은 개별적으로 animate 될 것이다. 우리는 `hero.state` expression 을 통해서 속성값을 바인딩 한다. 우리는 그것들이 항상 `inactive` 또는 `active` 에 있다는 것을 예상할 수 있다. 우리가 animation 상태를 정의했을때 부터 말이다.

이 설정으로,  hero object state 가 변경될 때마다 animate 하는 트랜지션이 보여진다. 이것은 전체 컴포넌트 구현체이다.

```
import {
  Component,
  Input,
  trigger,
  state,
  style,
  transition,
  animate
} from '@angular/core';
import { Heroes } from './hero.service';

@Component({
  moduleId: module.id,
  selector: 'hero-list-basic',
  template: `
    <ul>
      <li *ngFor="let hero of heroes"
          @heroState="hero.state"
          (click)="hero.toggleState()">
        {{hero.name}}
      </li>
    </ul>
  `,
  styleUrls: ['hero-list.component.css'],
  animations: [
    trigger('heroState', [
      state('inactive', style({
        backgroundColor: '#eee',
        transform: 'scale(1)'
      })),
      state('active',   style({
        backgroundColor: '#cfd8dc',
        transform: 'scale(1.1)'
      })),
      transition('inactive => active', animate('100ms ease-in')),
      transition('active => inactive', animate('100ms ease-out'))
    ])
  ]
})
export class HeroListBasicComponent {
  @Input() heroes: Heroes;
}

```
