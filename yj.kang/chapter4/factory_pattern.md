## Chapter 4

팩토리 패턴은 생성 패턴 중 하나입니다.

### 생성패턴

- 인스턴스를 만드는 절차를 추상화하는 패턴
- 객체를 생성, 합성하는 방법이나 객체의 표현 방법을 시스템과 분리해준다.
- 시스템이 상속(inheritance) 보다 복합(composite) 방법을 사용하는 방향으로 진화되어 가면서 더 중요해지고 있다.

##### 생성 패턴 두 가지 이슈

- 시스템이 어떤 Concrete Class를 사용하는지에 대한 정보를 캡슐화한다.
- 이들 클래스의 인스턴스들이 어떻게 만들고 어떻게 결합하는지에 대한 부분을 완전히 가려준다.
- 즉 무엇이 생성되고, 누가 이것을 생성하며, 이것이 어떻게 생성되는지, 언제 생성할 것인지 결정하는 데 유연성을 확보할 수 있게 된다.

### Factory Pattern

객체를 생성하는 인터페이스는 미리 정의하되 인스턴스를 만들 클래스의 결정은 서브 클래스 쪽에서 내리는 패턴

- 여러 개의 서브 클래스를 가진 슈퍼 클래스가 있을 때 인풋에 따라 하나의 자식 클래스의 인스턴스를 리턴해주는 방식

###### 활용성

- 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측할 수 없을 때
- 생성할 객체를 기술하는 책임을 자신의 서브클래스가 지정했으면 할 때

###### https://www.patterns.dev/

팩토리 패턴을 사용하면 팩토리 함수를 사용하여 새 객체를 만들 수 있습니다.

함수가 `new` 키워드를 사용하지 않고 새 객체를 반환하면 팩토리 함수입니다.

많은 유저의 정보가 필요한 서비스의 경우 `first_name`, `last_name`, `email` 등의 속성을 사용하여 사용자를 만들 수 있다고 가정했을 때,

팩토리 함수는 새로 생성된 객체에 fullName 프로퍼티를 추가하여 이름과 성을 반환하게 할 수 있다.

```js
const createUser = ({ firstName, lastName, email }) => ({
  firstName,
  lastName,
  email,
  fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
});

const user1 = createUser({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john@doe.com',
});

const user2 = createUser({
  firstName: 'Jane',
  lastName: 'Doe',
  email: 'jane@doe.com',
});

console.log(user1); // {firstName: "John", lastName: "Doe", email: "john@doe.com", fullName: ƒ fullName()}
console.log(user2); // {firstName: "Jane", lastName: "Doe", email: "jane@doe.com", fullName: ƒ fullName()}
```

팩토리 패턴은 비교적 복잡하고 구성 가능한 객체를 생성할 때 유용할 수 있는데, 키와 값이 특정 환경이나 구성에 따라 달라질 수 있을 때, 팩토리 패턴을 사용하면 사용자 정의 키와 값을 포함하는 새 객체를 쉽게 만들 수 있다.

```js
const createObjectFromArray = ([key, value]) => ({
  [key]: value,
});

createObjectFromArray(['name', 'John']); // { name: "John" }
```

##### 장점

- 동일한 속성을 공유하는 작은 객체를 여러 개 만들어야 할 때 유용하다.
- 현재 환경이나 사용자별 구성에 따라 사용자 지정 객체를 쉽게 반환할 수 있다.

##### 단점

- 자바스크립트에서 팩토리 패턴은 `new` 키워드를 사용하지 않고 객체를 반환하는 함수에 지나지 않는다.
- ES6 `Arrow Function`을 사용하면 매번 암시적으로 객체를 반환하는 작은 팩토리 함수를 만들 수 있다.

매번 새 객체를 생성하는 대신 다양한 경우에 새 인스턴스를 생성하는 것이 메모리 효율이 더 높을 수 있다.

```js
class User {
  constructor(firstName, lastName, email) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.email = email;
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

const user1 = new User({
  firstName: 'John',
  lastName: 'Doe',
  email: 'john@doe.com',
});

const user2 = new User({
  firstName: 'Jane',
  lastName: 'Doe',
  email: 'jane@doe.com',
});
```

###### 추가 정리

#### 암시적 반환 (Implicit Return)

- return 문 없이 값을 반환하는 경우에 암시적으로 undefined를 반환한다.
- 함수 내부에서 마지막으로 실행된 표현식의 결과값을 자동으로 반환 하기도 한다.

```js
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  a * b;
}

console.log(add(1, 2)); // 3
console.log(multiply(3, 4)); // undefined
```

add 함수에서는 return 문을 사용하여 명시적으로 반환값을 지정했지만, multiply 함수에서는 단순히 두 값을 곱한 결과값을 구하였다. return 문이 없으므로 undefined를 반환한다.

#### 명시적 반환 (Explicit Return)

- return 문을 사용하여 명시적으로 반환값을 지정할 수 있다.
- 함수 내부에 return 문을 실행하면 그 위치에서 함수의 실행이 종료되고 해당 반환값이 호출자에게 전달된다.

```js
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

console.log(add(1, 2)); // 3
console.log(multiply(3, 4)); // 12
```

#### new 생성자

- 함수를 생성자로 사용하여 객체를 생성할 수 있다.
- new 연산자를 사용하면 객체가 생성되고 생성된 객체가 this로 바인딩 되어 반환된다.

```js
function Product(name, price) {
  this.name = name;
  this.price = price;
}

let book = new Product('Book', 10);

console.log(book); // {name: "Book", price: 10}
```

Product 함수를 생성자로 사용하여 book 객체를 생성한다.
`new` 연산자를 사용하여 생성된 book 객체는 name과 price라는 두 개의 속성을 가지게 된다.

암시적 반환과 명시적 반환은 함수에서 반환값을 결정하는 방식에 대한 차이점이 있고, `new` 생성자는 객체를 생성하는 방식에 대한 차이점이 있다.

---

###### 추가 정리

1. 객체 생성에 대한 유연성이 필요한 경우
2. 코드의 중복을 줄이기 위해 객체 생성 로직을 하나로 통일해야 하는 경우
3. 생성되는 객체의 유형이 다양한 경우

- jQuery에서는 $(selector) 함수를 사용하여 셀렉터에 맞는 DOM 요소를 선택하고, 새로운 jQuery 객체를 반환하는데, 이는 내부적으로 팩토리 패턴을 활용
- React에서는 JSX를 사용하여 UI 컴포넌트를 생성하고, 내부적으로 이를 팩토리 함수로 변환하여 렌더링하는 것도 팩토리 패턴을 활용한 예시 중 하나
