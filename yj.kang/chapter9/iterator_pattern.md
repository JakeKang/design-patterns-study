## Chapter 9

### Iterator Pattern

컬렉션(collection)의 요소들을 순회(traversal)하면서 하나씩 처리하는 객체를 제공하는 패턴

- 순회하고자 하는 컬렉션의 내부 구조를 숨시고, 각 요소에 접근할 수 있는 인터페이스를 제공한다.

---

#### 구현방법

1. 컬렉션(collection) 객체를 생성한다.
   - 이 컬렉션은 일반적으로 배열, 맵, 셋 등의 자료구조를 사용
2. 이터레이터(iterator) 객체를 생성한다.
   - 이 객체는 다음 요소에 접근하는 next() 메서드와, 현재 요소를 반환하는 current() 메서드를 제공
3. 이터레이터 객체를 사용하여 컬렉션의 요소를 순회한다.
   - 이때 next() 메서드를 호출하여 다음 요소로 이동하고, current() 메서드를 호출하여 현재 요소를 가져온다.
4. 모든 요소를 순회할 떄까지 3번 과정을 반복한다.

이터레이터 패턴을 사용하면 컬렉션의 내부 구조에 대한 지식이 없이 요소에 접근할 수 있으며, 컬렉션 객체와 이터레이터 객체 간의 결합도를 낮출 수 있다.

또한 이터레이터 객체는 순회하고자 하는 컬렉션의 종류에 상관없이 일관된 인터페이스를 제공하기 때문에, 여러 종류의 컬렉션에 대해 동일한 알고리즘을 적용할 수 있다.

---

#### 장점과 단점

##### 장점

1. 내부 구현을 추상화하여 복잡성을 줄인다.
   - 컬렉션 내부의 구현을 추상화하여 순회하는 방법만 노출하므로, 컬렉션의 내부 구현을 바꾸더라도 이터레이터 객체를 사용하는 코드는 변경하지 않아도 된다.
   - 이는 유연성과 확장성을 높여준다.
2. 일관된 인터페이스를 제공한다.
   - 이터레이터 객체는 next() 메서드와 current() 메서드를 통해 일관된 인터페이스를 제공하므로, 여러 종류의 컬렉션에 대해 동일한 알고리즘을 적용할 수 있다.
3. 지연 평가(lazy evaluation)를 구현할 수 있다.
   - 컬렉션을 순회하면서 필요한 시점에서만 요소를 처리할 수 있도록 구현할 수 있다.
   - 이는 메모리 사용량을 최적화하고 성능을 개선하는데 도움이 된다.

##### 단점

1. 추가적인 객체 생성이 필요하다.
   - 이터레이터 객체를 생성하기 위해 추가적인 객체 생성이 필요한데 이때 메모리 사용량을 늘릴 수 있다.
2. 이터레이터 객체의 상태를 관리해야 한다.
   - 이터레이터 객체를 사용하기 위해서는 상태를 관리해주어야 한다. 이때 코드의 복잡도를 높일 수 있다.

---

#### 예제1

```js
// 이터레이터 객체 생성을 위한 클래스 정의
class MyIterator {
  constructor(data) {
    this.index = 0;
    this.data = data;
  }

  // 이터레이터 객체의 next 메서드 구현
  next() {
    if (this.index < this.data.length) {
      return { value: this.data[this.index++], done: false };
    } else {
      return { done: true };
    }
  }
}

// 이터러블 객체 생성을 위한 클래스 정의
class MyIterable {
  constructor(data) {
    this.data = data;
  }

  // 이터러블 객체의 Symbol.iterator 메서드 구현
  [Symbol.iterator]() {
    return new MyIterator(this.data);
  }
}

// 이터러블 객체 생성
const myIterable = new MyIterable(['one', 'two', 'three']);

// for...of 루프를 사용하여 순회
for (let value of myIterable) {
  console.log(value);
}
```

`MyIterator` 클래스 : 이터레이터 객체를 생성하기 위해 사용되는 클래스

- 생성자에서 인덱스와 데이터를 초기화하고, next 메서드를 구현하여 이터레이터 객체의 동작을 정의
- next 메서드는 현재 요소의 값을 value 속성에 담아서 반환
- 이후 인덱스를 증가시켜 다음 요소를 가리키도록 한다.
- 만약 다음 요소가 없다면 done 속성을 true로 설정하여 순회 종료.

`MyIterable 클래스` : 이터러블 객체를 생성하기 위해 사용되는 클래스

- 생성자에서 데이터를 초기화하고, Symbol.iterator 메서드를 구현하여 이터러블 객체의 동작을 정의한다.

`Symbol.iterator 메서드` : 이터러블 객체가 이터레이터 객체를 생성할 수 있는 메서드

- `Myiterator` 클래스의 인스턴스를 생성하여 반환

이터러블 객체를 생성한 후, `for...of` 루프를 사용하여 순회한다.
이때, `for...of` 루프는 내부적으로 `Symbol.iterator` 메서드를 호출하여 이터레이터 객체를 생성하고, `next` 메서드를 호출하여 순회한다.

---

#### 추가 예제

###### 배열을 순회하며 짝수만 출력하는 예제

```js
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

for (const number of numbers) {
  if (number % 2 === 0) {
    console.log(number);
  }
}
```

- 배열 `numbers`를 `for...of` 루프를 사용하여 순회
- `for...of` 루프는 내부적으로 배열의 이터레이터 객체를 생성하여 사용
- 이터레이터 객체는 배열의 요소를 하나씩 반환하며, 반환된 요소가 짝수인 경우에만 `console.log` 함수를 사용하여 출력

###### 간단한 문자열 처리기를 만드는 예제

- 문자열을 입력받아, 공백으로 분리하여 각 단어의 길이를 출력

```js
function wordLengths(str) {
  const words = str.split(' ');
  const iterator = words[Symbol.iterator]();
  let result = '';

  for (const word of iterator) {
    result += `${word.length} `;
  }

  return result.trim();
}

console.log(wordLengths('The quick brown fox jumps over the lazy dog')); // 출력 결과: "3 5 5 3 5 4 3 4 3"
```

- 입력받은 문자열을 `split` 메서드를 사용하여 공백으로 분리하여 배열 `words`에 저장
- `Symbol.iterator`를 사용하여 `words` 배열의 이터레이터 객체를 생성
- 이터레이터 객체는 `for...of` 루프에서 사용

- `for...of` 루프는 이터레이터 객체에서 `next` 메서드를 호출하여 요소를 하나씩 추출
- `next` 메서드는 `{value, done}` 형태의 객체를 반환
- `value`는 추출된 요소를 나타내며, `done`은 이터레이터 객체가 더 이상 반환할 요소가 없는지를 나타낸다.

- `for...of` 루프에서 각 단어의 길이를 구하여 `result` 문자열에 추가
- 마지막으로, `result` 문자열을 반환, `trim` 메서드를 사용하여 문자열 앞뒤 공백을 제거

---

### 이터레이터 패턴과 컴포지트 패턴

이터레이터 패턴과 컴포지트 패턴은 모두 객체의 구조를 조작하는 디자인 패턴

#### 공통점

- 객체 구조를 조작하는 디자인 패턴
- 복합 객체를 다루는 데에 효과적
- 객체 간의 관계를 트리구조로 표현할 수 있다.

#### 차이점

- 이터레이터 패턴은 객체를 순회하며 요소를 하나씩 추출하는 패턴
- 컴포지트 패턴은 복합 객체를 하나의 객체처럼 다루는 패턴

- 이터레이터 패턴은 순회하는 객체의 요소를 추출하여 처리하는 데에 중점
- 컴포지트 패턴은 복합 객체를 조작하고 관리하는 데에 중점

- 이터레이터 패턴은 자료구조에서 주로 사용되며, 이터러블한 객체를 순회한다.
- 컴포지트 패턴은 객체 간의 계층적인 관계를 다루며, UI 컴포넌트 등에서 주로 사용된다.

---

### 추가 정리

#### Symbol

- `ES6(ECMAScript 2015)`부터 추가된 원시 데이터 타입
- 객체의 고유한 식별자를 만들기 위해 사용된다.
- `string`, `number`, `boolean`, `null`, `undefined`, `object` 등과 같은 원시데이터 타입 중에서도 유일하게 고유한 값을 갖는 데이터 타입
  - 값이 생성될 때마다 새로운 고유 식별자가 생성되기 때문
  - 고유 식별자는 외부에서 참조할 수 없으며, 오직 값을 생성한 객체 내부에서만 사용된다.

###### 용도

주로 객체의 프로퍼티 키(Key)로 사용된다.
이를 통해 프로퍼티의 고유성을 보장할 수 있다.

내장 객체에서 `Symbol`을 사용하여 특정 기능을 구현하기도 한다.
위 예제의 `Symbol.iterator`는 이터러블 객체를 순회할 때 사용되는 이터레이터 객체를 생성하기 위한 메서드

```js
const mySymbol = Symbol();
const myObject = {};

myObject[mySymbol] = 'Hello, World!';

console.log(myObject[mySymbol]); // 출력 결과: "Hello, World!"
```

- Symbol 값을 생성하여 mySymbol 변수에 할당
- myObject 객체에 mySymbol을 프로퍼티 키로 사용하여 값을 할당
- myObject[mySymbol]을 통해 프로퍼티 값을 출력
- mySymbol 값이 고유하기 때문에 다른 객체에서는 해당 값을 참조할 수 없다.

---

#### 이터러블(Iterable) 객체

ES6에서 도입된 개념

- 이터레이터(Iterator)를 반환하는 [Symbol.iterator] 메서드를 구현하는 객체
- 이터레이터는 순회 가능한 데이터 소스로부터 값을 하나씩 순차적으로 반환하는 객체이며, next() 메서드를 통해 값을 순차적으로 반환
- 이터러블한 객체는 배열(Array), 문자열(String), 맵(Map), 셋(Set) 등과 같은 내장 객체를 비롯해 사용자 정의 객체에서도 구현이 가능
- 이터러블한 객체는 for...of 루프나 스프레드 문법 등에서 사용될 수 있다.

```js
const arr = [1, 2, 3];
for (const num of arr) {
  console.log(num);
}
```

- arr은 이터러블한 객체로, 배열의 각 요소를 하나씩 순회하며 출력합니다. 이를 가능하게 하는 것이 [Symbol.iterator] 메서드를 구현하는 것
