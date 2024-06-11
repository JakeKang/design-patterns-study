## Chapter 12

### Compound Pattern

- 둘 이상의 디자인 패턴을 조합하여 복잡한 문제를 해결하는 방법
- 여러 개의 문제를 해결하는 더 완전한 솔루션을 만들기 위해 두 개 이상의 디자인 패턴을 조합하는 것을 의미

- Model-View-Controller(MVC) 패턴

  - Observer 패턴과 Strategy 패턴을 결합한 컴파운드 패턴
  - Observer 패턴은 모델에 대한 변경 사항이 발생할 때 뷰를 알리는 데 사용되고, Strategy 패턴은 서로 다른 컨트롤러가 동일한 모델과 다른 방식으로 상호 작용할 수 있도록 해준다.

- Decorator-Factory 패턴

  - 런타임에서 객체에 새로운 기능을 추가할 수 있게 해주는 Decorator 패턴과, 객체가 생성될 때 정확한 객체 클래스를 지정하지 않고 객체를 생성할 수 있게 해주는 Factory 패턴을 결합

- 컴파운드 패턴은 단일 디자인 패턴만 사용하는 것보다 더 완전하고 견고한 솔루션을 제공

### 장점과 단점

#### 장점

1. 여러 디자인 패턴을 결합하여 복잡한 문제에 대한 보다 완벽하고 강력한 솔루션을 제공
2. 여러 패턴을 함께 사용하여 다양한 문제를 해결할 수 있으므로 솔루션을 보다 유연하게 설계할 수 있다.
3. 여러 패턴을 사용하면 우려 사항과 책임을 명확하게 구분할 수 있으므로 코드 가독성과 유지 관리 용이성을 향상시킬 수 있다.

#### 단점

1. 특히 신중하게 사용하지 않을 경우 코드 복잡성이 증가하고 이해하기 어려워질 수 있다.
2. 여러 디자인 패턴에 대한 심층적인 이해가 필요하므로 경험이 적은 개발자에게는 어려울 수 있다.
3. 너무 많은 패턴을 사용하면 과도한 엔지니어링으로 이어져 불필요한 복잡성과 성능 저하로 이어질 수 있다.

### 구성요소

자바스크립트에서 복합 패턴의 구성 요소는 결합되는 특정 패턴에 따라 달라진다.

일반적으로 복합 패턴에는 사용되는 각 개별 패턴의 구성 요소와 해당 패턴을 함께 통합하는 데 필요한 추가 구성 요소가 포함

예를 들어 관찰자 패턴과 전략 패턴을 결합하여 모델-뷰-컨트롤러 패턴을 생성하는 경우 구성 요소에 포함되는 요소는 아래와 같다.

Observer 패턴

- 주체: 종속 요소의 목록을 유지하고 상태 변경을 자동으로 알리는 개체
- 관찰자: 관찰자에게 변경 사항을 알리는 데 사용되는 방법을 정의하는 인터페이스
  <br/>

Strategy 패턴

- 컨텍스트: 전략 객체에 대한 참조를 유지하고 작업을 위임하는 객체
- 전략: 알고리즘을 구현하는 메서드를 정의하는 인터페이스

Observer 및 Strategy 패턴을 통합하는 데 필요한 추가 구성 요소에는 상황에 따라 다른 전략 객체에 작업을 위임하기 위해 컨텍스트 객체를 사용하는 컨트롤러 객체가 포함될 수 있다.

전반적으로 복합 패턴의 특정 구성 요소는 결합되는 패턴과 해결하고자 하는 문제에 따라 달라진다.
핵심은 각 패턴의 구성 요소와 이를 어떻게 통합하여 보다 강력한 솔루션을 만들 수 있는지 신중하게 고려하는 것

#### MVC 패턴으로 알아보는 예제

```js
// Observer pattern components
class Subject {
  constructor() {
    this.observers = [];
  }

  addObserver(observer) {
    this.observers.push(observer);
  }

  removeObserver(observer) {
    this.observers = this.observers.filter((obs) => obs !== observer);
  }

  notify(data) {
    this.observers.forEach((observer) => observer.update(data));
  }
}

class Observer {
  update(data) {
    console.log('Received new data:', data);
  }
}

// Strategy pattern components
class Context {
  constructor(strategy) {
    this.strategy = strategy;
  }

  executeStrategy(data) {
    this.strategy.execute(data);
  }
}

class Strategy {
  execute(data) {
    console.log('Strategy 1 executing with data:', data);
  }
}

// MVC pattern components
class Model {
  constructor() {
    this.subject = new Subject();
    this.context = new Context(new Strategy());
  }

  setData(data) {
    this.context.executeStrategy(data);
    this.subject.notify(data);
  }
}

class View {
  constructor(model) {
    this.model = model;
    this.model.subject.addObserver(new Observer());
  }

  render() {
    console.log('Rendering view');
  }
}

class Controller {
  constructor(model, view) {
    this.model = model;
    this.view = view;
  }

  handleData(data) {
    this.model.setData(data);
    this.view.render();
  }
}

// Usage example
const model = new Model();
const view = new View(model);
const controller = new Controller(model, view);

controller.handleData('Some data');
```

- Observer 패턴은 모델 개체의 변경 사항을 View 개체에 알리는 데 사용
- Strategy 패턴은 모델 데이터를 처리하기 위한 다양한 알고리즘을 구현하는 데 사용.
- 컨텍스트 객체는 모델 객체와 전략 객체 사이의 중개자 역할

모델-뷰-컨트롤러 패턴은 모델, 뷰 및 컨트롤러 클래스에 의해 구현된다.

- Model은 애플리케이션 상태를 관리하고 변경 사항을 View에 알리는 역할.
- View는 모델에서 제공한 상태를 기반으로 UI를 렌더링하는 역할.
- Controller는 사용자 입력을 처리하고 그에 따라 모델과 뷰를 업데이트

Controller는 객체에서 `handleData` 메서드가 호출되면 제공된 데이터로 Model을 업데이트하고, Model은 이를 View에 알리고 적절한 전략을 실행하여 데이터를 처리하도록 트리거한다.
그러면 View는 업데이트된 상태를 기반으로 UI를 렌더링합니다.
