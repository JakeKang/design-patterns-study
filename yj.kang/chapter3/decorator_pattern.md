## Chapter 3

### Decorator Pattern (장식자 패턴)

- 주어진 상황 및 용도에 따라 어떤 객체에 책임(기능)을 동적으로 추가하는 패턴

  - 객체의 결합을 통해 기능을 동적으로 유연하게 확장

- 기본 기능에 추가할 수 있는 기능의 종류가 많은 경우에 각 추가 기능을 Decorator 클래스로 정의 한 후 필요한 Decorator 객체를 조합함으로써 추가 기능의 조합을 설계하는 방식
  - ex) 기본 도로 표시 기능에 차선 표시, 교통량 표시, 교차로 표시, 단속 카메라 표시의 4가지 추가 기능이 있을 때 추가 기능의 모든 조합은 15가지가 된다. -> 데코레이터 패턴을 이용하여 필요 추가 기능의 조합을 동적으로 생성할 수 있다.

![diagram](/assets/decorator-pattern.png)

Component : ConcreteComponent와 Decorator가 구현할 인터페이스.

- 두 객체를 동등하게 다루기 위해 존재함
- 클라이언트가 실제 클래스를 사용하는 부분

ConcreteComponent Decorate를 받을 객체

- 기본 기능을 구현하는 클래스

Decorator : Decorate를 할 객체의 추상 클래스

- 기능 추가를 할 객체는 이 객체를 상속받는다.
- 많은 수가 존재하는 구체적인 Decorator의 공통 기능을 제공

ConcreteDecorator : Decorator를 상속받아 구현할 다양한 기능 책에

- 기능들은 ConcreteComponent에 추가되기 위해 만들어 진다.

##### 장점

1. 기존 코드를 수정하지 않고도 데코레이터 패턴을 통해 행동을 확장시킬 수 있다.
2. 구성과 위임을 통해서 실행중에 새로운 행동을 추가할 수 있다.

##### 단점

1. 의미없는 객체들이 너무 많이 추가될 수 있다.
2. 데코레이터를 너무 많이 사용하면 코드가 필요 이상으로 복잡해질 수 있다.

##### 사용하기 좋은 상황

1. 클래스의 요소들을 계속해서 수정을 하면서 사용하는 구조가 필요한 경우
2. 여러 요소들을 조합해서 사용하는 클래스 구조인 경우

#### ES6 클래스 정리

> ES2015(ES6) 부터 클래스 문법이 도입되었습니다.
>
> 클래스처럼 동작하게끔 만들어주는 편의문법이라는 점에 유의.
>
> 내부적으로 기존 prototype 기반의 상속구조로 변환되기에 사용자가 작성한 클래스는 결국 new 키워드와 함께 호출되기를 기다리는 함수(function)으로 바뀌게 됨.

###### 추가 정보

- class와 extends 키워드를 이용하여 클래스 계통을 명확하게 표현
- 생성자(constructor)를 제외할 수 있는데, 이 경우 상부 클래스의 생성자가 호출 됨.
- new 키워드를 통해 생성된 인스턴스는 부모 생성자 호출을 통해 속성값을 지니게 된다.
- 생성자를 명시적으로 선언하고 싶다면, 전개연산자(Spread Operator) 구문을 이용하여 부모에 전달할 파라미터를 작성가능.

```js
class Duck extends Animal {
  constructor(...args) {
    super(...args);
  }
}
```

- 코드 재사용을 목적으로 상부 클래스 계통이 지나치게 많은 책임과 기능을 부여하게되면 미래의 변화에 대응할 유연성을 잃게된다.

- 필요한 행위를 클래스 계통과 상관없이 독립된 클래스로부터 얻어와 행위를 탑재할 수 있는 해결책으로 믹스인(Mixins)이 등장함.
  - 특정 기능(행위)만을 담당하는 클래스로 단독 사용이 아닌 다른 클래스에 탑재되어 사용될 목적으로 작성된 (조각) 클래스를 의미
  - Javascript의 경우 클래스 문법, prototype 기반 모델, Object.assign을 이용해 객체에 직접 행위를 붙이는 작업을 할 수 있다.

#### 데코레이터 패턴 예제

```js
// Decoratable 믹스인 클래스

const Decoratable = (superclass) =>
  class extends superclass {
    decorate(referenceName, decorator) {
      // reference 는 기존 메서드를 참조
      const reference = this[referenceName];

      // 오버라이딩을 통해 장식자 구현
      this[referenceName] = (...args) {
        // reference 메서드 연산수행 후, 결과를 첫번째 인자로 담아 decorator 호출
        return decorator.apply(this, [
          reference.call(this, ...args),
          ...args
        ])
      }
    }
  };

  // call(null, 1, 2, 3) -> 함수와 동일한 인자 추가, null은 this
  // apply(null, [1,2,3]) -> 인자를 하나로 묶어 배열로 만들어 넣는다.
  // bind -> this만 교체 하고 위 두개와 다르게 호출 하지 않음
```

- decorate 메서드는 기존 메서드 식별을 위한 referenceName과 이를 장식하는 decorator를 인자로 취함
- reference 는, this 인스턴스에서 referenceName과 매칭되는 기존 메서드에 대한 참조를 담고 있음.
- 기존 메서드를 오버라이딩하는 내부 구현부에 reference 함수의 실행결과는 decorator 함수가 실행될 때 첫번째 인자값으로 전달됨.

```js
// Decoratable 믹스인 클래스를 실제 상속구조에 넣고 실행

// super class
class PureElement {
  constructor(name) {
    this.name = name;
  }

  element() {
    return document.createElement('div');
  }
}

// BorderedElement는 PureElement와 DDecoratable을 상속
class BorderedElement extends Decoratable(PureElement) {
  constructor(name) {
    super(name);

    // Private
    const appendBorder = (element) => {
      element.style.border = '1px solid';
      return element;
    };

    // 실제 decorating 수행
    this.decorate('element', (decoObj) => {
      return appendBorder(decoObj);
    });
  }
}

new BorderedElement('borderDiv').element();

// <div style="border:1px solid"></div>
```

- BorderElement는 PureElement에서 유래되어, Decoratable 믹스인 클래스를 통해 Decorate 메서드를 상속받음.
- 생성자(constructor)안에서 상속받은 믹스인 클래스의 decorate 메서드가 사용됨
- decorate 메서드는 element메서드에 대한 장식을 수행하고 그 결과를 반환
- 추후 (런타임 포함)에 다시한번 장식이 필요할 때 현재의 결과를 전달받을 수 있음.

- appendBorder 함수는 Javascript 클래스문법에서 private를 선언할 수 있는 유일한 공간이 객체 생성자 내부뿐인데, 이때 public 메서드인 element는 생성자 내부에 선언된 함수에 접근할 수 없지만, 장식자 기법이 이를 가능하게 한다.

##### 장식자 패턴에서 이전 연산의 수행결과가 필요한 이유

- 본래 기존 연산에 덫붙이는 작업이 필요할 때 사용하는 디자인 패턴
- 기존 결과물에 상관없이 필요한 행위가 연달아 호출되는 구조로 사용될 수 도 있지만, 결과물의 반환은 이 후에 장식이 필요한 시점에 최종 장식 결과물을 전달해준다.

```js
// 최종 결과물을 전달받아 element 메서드를 다시한번 장식하는 과정
// decorate 는 public 메서드이기에 가능

let el = new BorderedElement('borderedDiv');
el.decorate('element', (decoObj) => {
  decoObj.style.background = '#eee';
  return decoObj;
});

el.element();

// <div style="border:1px solid #eee"></div>
```

---

###### 추가 정리

1. 객체의 기능을 동적으로 추가/삭제해야 하는 경우
2. 객체의 기능을 여러 가지 방식으로 조합해야 하는 경우
3. 상속으로 객체를 확장하기 어려운 경우 (상속 대신 데코레이터 객체를 사용하여 기능을 추가/삭제)

React에서는 컴포넌트에 데코레이터를 적용하여 컴포넌트의 기능을 동적으로 확장
Express.js에서는 미들웨어 함수를 데코레이터로 사용하여 HTTP 요청을 처리하는 라우터 함수의 기능을 확장
