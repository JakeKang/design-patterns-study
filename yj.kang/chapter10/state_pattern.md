## Chapter 10

### State Pattern

객체의 상태(state)에 따라 객체의 행동(behavior)을 변경하는 방법을 제공

객체의 상태를 클래스로 표현하고, 해당 상태에서 가능한 행동들을 각각의 클래스 내부에서 구현한다.
그리고 객체의 상태가 변화할 때마다, 해당 상태에 맞는 클래스의 행동을 수행하도록 객체를 전환한다.

객체의 행동을 상태에 따라 쉽게 변경할 수 있어서, 코드 유지보수성을 높이고 코드의 중복을 피할 수 있다.
또한 객체의 상태를 명확하게 표현할 수 있어서, 객체의 상태 변화를 추적하기 쉽다.

---

#### 구성요소

1. Context : 상태가 변화하는 객체

- 객체 내부에서는 현재 상태를 저장하고, 상태에 맞는 행동을 수행

2. State : 상태를 표현하는 인터페이스

- 상태에 따라 가능한 행동을 정의하고, 해당 상태에서 수행되는 메서드를 구현

3. ConcreteState : State 인터페이스를 구현하는 클래스

- 객체의 상태에 따라 가능한 행동을 구현한다.

---

#### 장점과 단점

##### 장점

1. 유연성 : 객체의 상태 변화에 따라 행동을 쉽게 변경할 수 있다.

- 객체의 유연성을 높이고, 코드 유지보수성을 높일 수 있다.

2. 코드 중복 제거 : 객체의 상태에 따라 수행하는 행동들을 클래스로 분리

- 코드 중복을 제거할 수 있다.

3. 객체 분리 : 객체의 상태를 클래스로 분리

- 이는 객체의 상태 변화를 추적하기 쉽게 만들어준다.

##### 단점

1. 구현 복잡도

- 객체의 상태마다 클래스를 만들어야 하기 때문이다.

2. 클래스 수 증가

- 상태마다 클래스를 만들어야 하기 때문에, 클래스 수가 증가할 수 있다.
- 객체지향적인 설계를 위해 중요하지만, 클래스 수가 많아질수록 코드 가독성이 떨어질 수 있다.

객체의 상태 변화에 따라 행동을 변경할 때 유용하지만, 구현 복잡도와 클래스 수 증가 등의 단점을 고려해야 한다.

---

#### 구현 예제

```js
// State 인터페이스
class State {
  handle() {
    throw new Error('handle 메서드를 구현해야 합니다.');
  }
}

// ConcreteState 클래스
class StateA extends State {
  handle() {
    console.log('StateA의 handle 메서드를 호출했습니다.');
  }
}

class StateB extends State {
  handle() {
    console.log('StateB의 handle 메서드를 호출했습니다.');
  }
}

// Context 클래스
class Context {
  constructor(state) {
    this.state = state;
  }

  setState(state) {
    this.state = state;
  }

  request() {
    this.state.handle();
  }
}

// 예시 코드 실행
const context = new Context(new StateA());
context.request(); // "StateA의 handle 메서드를 호출했습니다."
context.setState(new StateB());
context.request(); // "StateB의 handle 메서드를 호출했습니다."
```

State 인터페이스를 정의하고, StateA와 StateB 클래스가 이를 구현한다.
Context 클래스는 현재 상태를 저장하고, 상태에 따라 수행할 메서드를 호출한다.

위 예시 코드에서는 StateA와 StateB 클래스의 handle 메서드가 호출된다.

---

#### 실사용에 적용 가능한 예제

실사용에 적용 가능한 예제 중 하나는 상태 기반 UI 구현이있다.

예로, 온라인 쇼핑몰에서 상품 주문 절차를 구현할 때, 주문 절차에 따라 다양한 상태가 발생한다.

- 주문 폼을 작성하는 단계, 배송 주소를 입력하는 단계, 결제를 완료하는 단계

이때, 각각의 상태에 따라 UI가 다르게 보여야 한다.

스테이트 패턴을 이용하여 각각의 상태에 해당하는 클래스를 만들어서, 상태가 변경될 때마다 해당 클래스의 메소드를 호출하여 UI를 변경할 수 있다.

또 다른 예시로, 게임 개발에서 상태 변화에 따라 행동을 변경할 때 사용할 수 있다.

- 캐릭터가 걷는 상태, 뛰는 상태, 공격하는 상태 등에 따라 다양한 행동이 발생한다.

이때, 각각의 상태에 따른 클래스를 만들어서, 상태가 변경될 때마다 해당 클래스의 메소드를 호출하여 캐릭터의 행동을 변경할 수 있다.

---

#### 예제 2

신호등 시스템

```js
class TrafficLight {
  constructor() {
    this.states = [new GreenLight(), new YellowLight(), new RedLight()];
    this.current = this.states[0];
  }

  change() {
    const totalStates = this.states.length;
    let currentStateIndex = this.states.findIndex(
      (state) => state === this.current,
    );
    if (currentStateIndex + 1 < totalStates) {
      this.current = this.states[currentStateIndex + 1];
    } else {
      this.current = this.states[0];
    }
    console.log(this.current.constructor.name);
  }

  start() {
    setInterval(() => {
      this.change();
    }, 2000);
  }
}

class GreenLight {
  constructor() {
    console.log('Green Light');
  }
}

class YellowLight {
  constructor() {
    console.log('Yellow Light');
  }
}

class RedLight {
  constructor() {
    console.log('Red Light');
  }
}

const trafficLight = new TrafficLight();
trafficLight.start();
```

- TrafficLight 클래스 : 교통 신호등의 상태
- TrafficLight 상태 클래스 : GreenLight, YellowLight, RedLight를 정의

- 현재 상태를 나타내는 current 멤버 변수와 현재 상태를 변경하는 change 메소드를 가지고 있다.

- start 메소드는 일정 시간마다 change 메소드를 호출하여 신호등의 색을 변경한다.

- 각각의 상태 클래스는 생성자에서 해당 상태의 이름을 출력한다. change 메소드에 현재 상태를 출력하도록 구현되어있다.

- 일정 시간마다 TrafficLight의 상태가 변경되며, 색이 변경됨에 따라 해당 상태 클래스의 이름이 출력된다.
