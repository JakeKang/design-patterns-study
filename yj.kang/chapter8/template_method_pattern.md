## Chapter 7

### Template Method Pattern Pattern

---

알고리즘의 구조를 정의하고 구현할 때 사용할 수 있다.
상위 클래스에서 알고리즘의 구조를 정의하고 하위 클래스에서 구체적인 구현을 처리하는 방식으로 동작

여러 클래스에서 공통적으로 수행되는 알고리즘의 일부를 추상화시켜서 상위 클래스에 두고, 하위 클래스에서는 상위 클래스에서 정의한 메서드를 오버라이딩하여 구체적인 내용을 구현한다.

---

#### 구조

- AbstractClass : 핵심이 되는 추상 클래스로, 알고리즘의 구조를 정의한다.
  - 이 클래스는 추상 메서드와 구체적인 메서드로 이루어져 있다.
  - 추상 메서드는 하위 클래스에서 구체적인 구현을 처리하도록 하고, 구체적인 메서드는 하위 클래스에서 공통적으로 사용되는 메서드를 정의한다.
- ConcreteClass : AbstractClass를 상속받아 구현하는 구체적인 클래스.
  - 추상 메서드를 구현하고, 구체적인 메서드를 오버라이드 한다.

---

#### 장점과 단점

##### 장점

- 알고리즘의 구조와 구현을 분리할 수 있어, 코드의 재사용성과 유지보수성이 향상된다.
- 상위 클래스에서 공통적으로 사용되는 메서드를 정의함으로써 코드의 일관성이 향상된다.
- 상위 클래스에서 알고리즘의 구조를 정의하므로, 하위 클래스에서 오버라이드하는 메서드의 개수가 줄어든다.

##### 단점

- 상위 클래스에서 알고리즘의 구조를 정의하기 때문에, 하위 클래스에서 알고리즘의 구조를 변경하기 어렵다.
- 상위 클래스에서 구현한 메서드가 하위 클래스에서 사용되지 않을 경우, 코드의 중복이 발생할 수 있다.

---

#### 예제 1

```js
class Pizza {
  prepare() {
    this.prepareDough();
    this.addToppings();
    this.bake();
  }

  prepareDough() {
    console.log('Preparing dough');
  }

  addToppings() {
    console.log('Adding toppings');
  }

  bake() {
    console.log('Baking pizza');
  }
}

class CheesePizza extends Pizza {
  addToppings() {
    console.log('Adding cheese toppings');
  }
}

class PepperoniPizza extends Pizza {
  addToppings() {
    console.log('Adding pepperoni toppings');
  }
}

const cheesePizza = new CheesePizza();
cheesePizza.prepare();

const pepperoniPizza = new PepperoniPizza();
pepperoniPizza.prepare();
```

- `Pizza` 클래스 : 피자를 만들기 위한 골격을 정의
- `CheesePizza`와 `PepperoniPizza` 클래스 : 상위 클래스에서 정의한 메서드를 오버라이딩하여 각각의 토핑을 추가하는 구체적인 내용을 구현

각각의 피자를 만들 때는 `prepare()` 메서드를 호출하면, 상위 클래스에서 정의한 알고리즘에 따라 피자가 만들어진다.

---

#### 예제 2

커피와 차를 만드는 과정에서 공통적인 작업인 물 끓이기, 컵에 따르기 등을 상위 클래스에서 정의하고, 각 음료에 맞게 하위 클래스에서 오버라이드하여 구현

```js
// 상위 클래스
class Beverage {
  prepare() {
    this.boilWater();
    this.brew();
    this.pourInCup();
  }

  boilWater() {
    console.log('물을 끓입니다.');
  }

  brew() {
    throw new Error('서브클래스에서 brew 메서드를 구현해야 합니다.');
  }

  pourInCup() {
    console.log('컵에 따르는 중입니다.');
  }
}

// 하위 클래스: 커피
class Coffee extends Beverage {
  brew() {
    console.log('필터로 커피를 우려내는 중입니다.');
  }
}

// 하위 클래스: 차
class Tea extends Beverage {
  brew() {
    console.log('차를 우려내는 중입니다.');
  }
}

// 커피와 차를 만드는 코드
const coffee = new Coffee();
coffee.prepare();

const tea = new Tea();
tea.prepare();
```

`Beverage` 클래스 : `prepare` 메서드를 정의

- 공통적인 작업인 물 끓이기, 컵에 따르기 등을 수행
- `brew` 메서드는 추상 메서드로 선언
  - 이 추상 메서드는 하위 클래스에서 오버라이드하여 각 클래스에 맞게 구현

하위 클래스

- `Coffee`와 `Tea` 클래스 : `brew` 메서드를 오버라이드하여 커피와 차를 우려내는 작업을 수행

1. 커피와 차를 만드는 코드에서는 각 음료의 클래스를 생성한 후, `prepare` 메서드를 호출
2. 상위 클래스에서 정의한 `prepare` 메서드가 실행되어 공통적인 작업을 수행하고, 하위 클래스에서 오버라이드한 `brew` 메서드가 실행

---

#### 실사용 예제

게임 개발이나 프레임워크 개발 시 예제

게임 개발 : 여러 종류의 캐릭터가 있을 때, 캐릭터마다 공통된 기능인 이동, 공격, 방어 등이 있습니다. 이때, 이동, 공격, 방어 등의 메서드를 상위 클래스에서 정의하고 각 캐릭터의 클래스에서는 해당 메서드를 오버라이드하여 각 캐릭터에 맞게 구현할 수 있다.

프레임워크 개발 : 프레임워크에서는 특정 작업을 수행하는 메서드를 구현한 다음, 이를 상위 클래스에서 호출한다. 하위 클래스에서는 이 메서드를 오버라이드하여 각 클래스에 맞게 구현한다. 이렇게 하면 프레임워크를 사용하는 개발자는 상위 클래스의 메서드를 호출하기만 하면 된다.
