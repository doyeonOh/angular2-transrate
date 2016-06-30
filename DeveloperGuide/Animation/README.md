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
- 상태와 트랜지션
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
![enter image description here](https://angular.io/resources/images/devguide/animations/animation_basic_click.gif)

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

상태와 트랜지션
-

Angular Animation 들은 논리적 상태나 상태 사이에 있는 트랜지션 관점에서 정의된다.

Animation 상태는 우리가 application code 로 정의할 수 있는 string value 이다. 위의 예제에서, 우리는 hero object 의 논리적 상태를 기반으로 `'active'` 와 `'inactive'` 라는 상태를 사용했다. 상태의 소스는 이번 경우에 간단한 객체 속성일 수도 있고 메소드에서 계산된 값일수도 있다.  중요한 점은 우리가 component 의 template 을 통해서 그것을 읽을 수 있다는 것이다.

우리는 각 animation 상태에 *styles* 정의할 수 있다.

```
state('inactive', style({
  backgroundColor: '#eee',
  transform: 'scale(1)'
})),
state('active',   style({
  backgroundColor: '#cfd8dc',
  transform: 'scale(1.1)'
})),
```
저기있는 `state` 정의들은 각 상태의 *end styles*  지정하고 있다.  그것들은 그 상태로 트랜지션 되었을 때 element 를 한번 적용한다. 그리고 그 상태로 남아있는 한 유지된다. 그런 의미에서 상태가 많아지면 우리는 이곳에 있는 것보다 animation 을 더 많이 정의해야 한다. 우리는 실제로 서로 다른 상태에 있는 element 의 style 을 정의하는 것이다.

우리가 상태들을 가지고 있을 때, 그 상태 사이의 *트랜지션*을 정의할 수 있다. 각 트랜지션은 하나의 스타일 셋과 다음 사이에서 스위칭 타이밍을 컨트롤 할 수 있다.

```
transition('inactive => active', animate('100ms ease-in')),
transition('active => inactive', animate('100ms ease-out'))
```
![enter image description here](https://angular.io/resources/images/devguide/animations/ng_animate_transitions_inactive_active.png)

만약 우리가 몇몇개의 트랜지션에 대해 같은 타이밍의 설정을 가지고 있으면, 우리는 같은 `transition` 정의을 통해 그것들을 합칠 수 있다.

```
transition('inactive => active, active => inactive',
 animate('100ms ease-out'))
```

우리가 트랜지션의 양방향이 같은 타이밍을 가지고 있을 때는, 이전 예제에서 했던 것과 같이, 우리는 `<==>` 속기 문법을 사용할 수 있다.

```
transition('inactive <=> active', animate('100ms ease-out'))
```

가끔씩 우리는 animation 시에서는 적용하지만 끝난 후 더이상 유지하지 않길 원하는 스타일들을 가지고 있다. 우리는 `transition` 안에서 그런 스타일을 inline 으로 정의할 수 있다. 이번 예제에서, element 는 즉시 하나의 스타일 셋을 받고 다음으로 애니메이션 된다. 트랜지션이 종료된 후에는, 스타일들이 아무것도 유지되지 않을 것이다. 왜냐하면 그것들이 `state` 에 정의되지 않았기 때문이다.

```
transition('inactive => active', [
  style({
    backgroundColor: '#cfd8dc',
    transform: 'scale(1.3)'
  }),
  animate('80ms ease-in', style({
    backgroundColor: '#eee',
    transform: 'scale(1)'
  }))
])
```

####와일드카드 상태 `*`

`*`( 와일드카드 ) 상태는 어떠한 animation 상태에도 매칭된다. 이것은 animation 안에 있는 어떤한 상태에도 관계없이 상태와 트랜지션을 정의함에 있어서 매우 유용하다. 예를 들어

- `active => *` 트랜지션은 element 상태가 `active` 에서 다른 어떤한 것으로 변경될 때 적용된다.
- `* => *` 트랜지션은 두개의 상태에서 어떠한 변경이라도 발생되면 적용된다.

![enter image description here](https://angular.io/resources/images/devguide/animations/ng_animate_transitions_inactive_active_wildcards.png)

#### `void` 상태

특별한 상태의 하나인 `void` 는 어떠한 animation 에도 적용할 수 있다. 그건 view 에 붙이지 않는 element 일 때 적용한다. 이것은 그것이 아직 추가하지 않거나, 혹은 삭제되었기 때문일지도 모른다. `void` 상태는 '진입' 과 '이탈' animation 을 정의할 때 유용하게 쓰인다.

예를 들어서 `* => void` 트랜지션은 이탈하기 전 상태와 관계 없이 view 를 이탈할 때 적용된다.
![enter image description here](https://angular.io/resources/images/devguide/animations/ng_animate_transitions_void_in.png)

와일드카드 상태인 `*` 은 또한 `void` 에 매칭된다.
