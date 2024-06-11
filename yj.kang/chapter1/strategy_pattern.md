## Chapter 1

#### 디자인 패턴이란?

- 스프트웨어 개발 과정에서 발견된 설계의 노하우를 축적하여 그 방법에 이름을 붙여서 이후에 재사용하기 좋은 형태로 특정 규약을 만들어서 정리한 것

- 설계에 있어 공통적인 문제들에 대한 표준적인 해법과 작명법을 제안하며, 알고리즘과 같이 프로그램 코드로 바로 변환될 수 있는 형태는 아니지만, 특정한 상황에서 구조적인 문제를 해결하는 방식

- "효율적인 코드를 만들기 위한 방법론"

---

### Strategy Pattern (전략 패턴)

- 각각의 알고리즘을 교환이 가능하도록 정의, 캡슐화를 한 다음, 서로 교환해서 사용할 수 있는 패턴

  - 각각을 캡슐화하여 교환해서 사용할 수 있다.
  - 클라이언트와는 독립적으로 알고리즘을 변경할 수 있다.
  - **행위**를 클래스로 캡슐화 하여 동적으로 행위를 바꾸어도 코드가 복잡하지 않도록 한다.
  - 전략을 쉽게 바꾸도록 해주는 디자인 패턴

- OCP에 위배되지 않고 시스템이 거대해졌을 때, 메소드의 중복을 해결할 수 있다.

###### OCP 란?

> OCP(Open Closed Principle)
> 객체지향 설계 5원칙 SOLID (SRP, OCP, LSP, ISP, DIP)
>
> 기존의 코드를 변경하지 않으면서 기능을 추가할 수 있도록 설계가 되어야 한다.
> 확장에 대해서는 개방적이고, 수정에 대해서는 폐쇄적 이어야한다.

###### Class 다이어그램

![diagram](/assets/strategy-pattern.png)

- Strategy : 인터페이스나 추상 클래스로 외부에서 동일한 방식으로 알고리즘을 호출하는 방법을 명시

- ConcreteStrategy : 스트래티지 패턴에서 명시한 알고리즘을 실제로 구현한 클래스

- Context : 스트래티지 메소드를 호출해서 사용하는 클래스

###### 인터페이스

> 인터페이스는 변수나 함수, 그리고 클래스가 만족해야하는 최소 규격을 지정할 수 있게 해주는 도구이다.
>
> 꼭 자바의 interface 구조를 지칭하는 것이 아닌 어떤 상위 형식에 맞춤으로 인해 다형성을 활용한다는 것

```typescript
interface Todo {
  id: number;
  content: string;
  completed: boolean;
}
```

###### 추상 클래스

> 공통된 행동, 필드를 묶어 하나의 클래스를 만드는 것

---

##### 예시 1

```js
// Strategy
function TicketPrice() {
  this.theaterChain;

  this.setTheaterChain = function (chain) {
    this.theaterChain = chain;
  };

  this.calculate = function (quantity) {
    return this.theaterChain.getTicketPrice(quantity);
  };
}

// ConcreteStrategyA
function CGV() {
  this.getTicketPrice = function (quantity) {
    return `${quantity * 14000}원`;
  };
}

// ConcreteStrategyB
function Megabox() {
  this.getTicketPrice = function (quantity) {
    return `${quantity * 17000}원`;
  };
}

// ConcreteStrategyC
function LotteCinema() {
  this.getTicketPrice = function (quantity) {
    return `${quantity * 15000}원`;
  };
}

// Context
const cgv = new CGV();
const megabox = new Megabox();
const lotteCinema = new LotteCinema();

const ticketPrice = new TicketPrice();

ticketPrice.setTheaterChain(cgv); // cgv
console.log(ticketPrice.calculate(1)); // 14000원

ticketPrice.setTheaterChain(megabox); // megabox
console.log(ticketPrice.calculate(2)); // 34000원

ticketPrice.setTheaterChain(lotteCinema); // lotteCinema
console.log(ticketPrice.calculate(3)); // 45000원
```

---

##### 예시 2

```js
class Shipping {
  constructor() {
    this.company = '';
  }

  setCompany(company) {
    return (this.company = company);
  }

  calculate(packageWeight) {
    return this.company.calculate(packageWeight);
  }
}

function Hanjin() {
  this.calculate = (packageWeight) => {
    return `${packageWeight * 1000}원`;
  };
}

function PostOffice() {
  this.calculate = (packageWeight) => {
    return `${packageWeight * 800}원`;
  };
}

function CJ() {
  this.calculate = (packageWeight) => {
    return `${packageWeight * 1500}원`;
  };
}

const init = () => {
  const package = { from: '0701', to: '0206', weight: 5 };

  const hanjin = new Hanjin();
  const postOffice = new PostOffice();
  const cj = new CJ();

  const shipping = new Shipping();

  shipping.setCompany(hanjin);
  console.log(shipping.calculate(package.weight));

  shipping.setCompany(postOffice);
  console.log(shipping.calculate(package.weight));

  shipping.setCompany(cj);
  console.log(shipping.calculate(package.weight));
};

init();
```

---

#### 예제 3

게임 개발에서 캐릭터의 움직임을 다양하게 구현할 때 많이 활용
예를 들어, 플레이어 캐릭터의 이동 방식은 보통 키보드나 마우스 등의 입력에 따라 달라진다.

```js
// 이동 전략을 담은 클래스
class MoveStrategy {
  move() {
    throw new Error('Subclass must implement abstract method');
  }
}

// 걷기 전략
class WalkStrategy extends MoveStrategy {
  move() {
    console.log('Walk forward');
  }
}

// 뛰기 전략
class RunStrategy extends MoveStrategy {
  move() {
    console.log('Run forward');
  }
}

// 플레이어 캐릭터 클래스
class Player {
  constructor(strategy) {
    this.strategy = strategy;
  }

  move() {
    this.strategy.move();
  }
}

// 게임에서 사용되는 예시 코드
const player = new Player(new WalkStrategy());
player.move(); // Walk forward

player.strategy = new RunStrategy();
player.move(); // Run forward
```

이동 방식을 담은 MoveStrategy 추상 클래스와 그 하위 클래스인 WalkStrategy와 RunStrategy를 만들었다.
이후 이동 방식을 선택할 때 Player 클래스의 인스턴스를 생성할 때 전략 객체를 인자로 전달하며, 이동할 때에는 move() 메서드를 호출한다.
Player 클래스의 내부 구현에서는 전달된 전략 객체의 move() 메서드를 호출하여 이동 방식을 구현

---

###### 추가 정리

1. UI 이벤트 핸들러

- UI 이벤트 핸들러에서 스트래티지 패턴을 사용하여 여러 가지 동작 중 하나를 선택하는 방법을 구현
- 사용자가 클릭하면 특정 동작이 실행되도록 선택

2. 데이터 정렬

- 데이터를 정렬하는 함수를 작성할 때 스트래티지 패턴을 사용하여 다양한 정렬 알고리즘을 적용
- 용자가 선택한 옵션에 따라 오름차순 또는 내림차순으로 데이터를 정렬하는 것이 가능

3. 데이터 검색

- 데이터 검색에 스트래티지 패턴을 사용하여 다양한 검색 알고리즘을 적용
- 사용자가 선택한 검색 기준에 따라 특정 데이터를 검색하는 것이 가능

4. 알고리즘의 동적 변경

- 알고리즘의 동적 변경이 필요한 상황에서 스트래티지 패턴을 사용
- 실시간으로 시스템의 부하를 모니터링하고, 성능이 저하될 때 알고리즘을 변경하여 처리 속도를 높일 수 있다.
