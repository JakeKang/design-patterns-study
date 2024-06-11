## Chapter 2

### Observer Pattern (옵저버 패턴)

- 객체의 상태변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴

- 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다.
- 발행/구독 모델로 알려져 있기도 하다.

> 어떤 객체의 상태가 변할 때 그와 연관된 객체 들에게 알림을 보내는 디자인 패턴

###### Observer(관찰자)

- 상태 변화를 감지하는 대상
- 옵저버에는 함수나 객체 모두 등록이 가능

###### Oberable(객체)

- 상태가 변경되는 대상
- subscribe, unsubscribe, notify 등 행동을 처리하는 메서드를 보유하고 있어야 한다.

> Observable에서 Observer로 한 방향으로 진행된다.
>
> 옵저버는 마치 구독 시스템의 push 알람처럼, 구독 대상의 변화에 의한 push 방식으로 데이터를 얻을 수 있다.
>
> 옵저버는 그저 구독 리스트에 등록만 되어 있다면, 옵저버블로부터 정보를 받아 볼 수 있는 것

#### 장점

1. 실시간으로 한 객체의 변경사항을 다른 객체에 전파할 수 있다.
2. 느슨한 결합으로 시스템이 유연하고 객체간의 의존성을 제거할 수 있다.

#### 단점

1. 너무 많이 사용하게 되면, 상태 관리가 힘들 수 있다.
2. 데이터 분배에 문제가 생기면 자칫 큰 문제로 이어질 수 있다.

---

##### 예시

```js
class Subject {
  constructor() {
    this.observers = [];
  }

  addObserver(observer) {
    this.observers.push(observer);
  }

  removeObserver(observer) {
    const removeIndex = this.observers.findIndex((obs) => {
      return observer === obs;
    });

    if (removeIndex !== -1) {
      this.observers = this.observers.slice(removeIndex, 1);
    }
  }

  notify(data) {
    if (this.observers.length > 0) {
      this.observers.forEach((observer) => observer.update(data));
    }
  }
}

export default Subject;
```

---

#### 예제 2

주로 이벤트 기반의 시스템에서 많이 사용되는 패턴
특정 객체의 상태가 변경될 때 다른 객체들에게 이를 알리고 싶은 경우에 사용할 수 있다.

```js
// Subject
class Button {
  constructor() {
    this.handlers = [];
  }

  subscribe(fn) {
    this.handlers.push(fn);
  }

  unsubscribe(fn) {
    this.handlers = this.handlers.filter(function (item) {
      if (item !== fn) {
        return item;
      }
    });
  }

  click() {
    this.handlers.forEach(function (item) {
      item();
    });
  }
}

// Observer
function log() {
  console.log('Button clicked');
}

// Usage
const button = new Button();
button.subscribe(log);
button.click(); // "Button clicked" 출력
```

Button 클래스가 주체(Subject)가 되고, subscribe, unsubscribe, click 메서드를 제공
subscribe 메서드는 옵저버 함수를 구독하며, unsubscribe 메서드는 옵저버 함수를 구독 해제
click 메서드는 버튼을 클릭할 때 옵저버 함수들을 호출

log 함수가 옵저버(Observer)가 되어 Button 객체의 subscribe 메서드를 호출하여 이벤트에 등록되고, click 메서드가 호출될 때마다 log 함수가 실행

---

###### 추가 정리

DOM 이벤트 핸들링, 비동기 프로그래밍, 데이터 바인딩 등에서 널리 사용

- DOM 이벤트 핸들링에서는 addEventListener를 사용하여 이벤트를 등록하고, 이벤트가 발생하면 등록된 콜백 함수가 실행

  - 이때 등록된 콜백 함수가 옵저버 역할을 하며, 이벤트가 발생하면 해당 이벤트에 대한 처리를 수행

- 비동기 프로그래밍에서는 Promise와 같은 비동기 처리 방식

  - Promise의 then 메서드는 이행(resolve)과 거부(reject) 콜백 함수를 받는데, 이 콜백 함수가 옵저버 역할

- 데이터 바인딩에서는 데이터의 변경을 감지하여 해당 변경 사항을 자동으로 반영
  - 이때 데이터 변경을 감지하는 객체가 옵저버 역할
  - ue.js나 AngularJS 같은 프론트엔드 프레임워크에서는 데이터 바인딩에 옵저버 패턴을 사용
