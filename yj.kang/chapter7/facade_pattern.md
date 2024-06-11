## Chapter 7

### Facade Pattern

---

복잡한 시스템 또는 서브시스템의 인터페이스에 간단한 인터페이스를 제공하는 패턴

- 외부에서 복잡한 내부 동작을 할 필요 없이 단순한 인터페이스를 사용하여 시스템에 접근할 수 있도록한다.

즉, 복잡한 코드나 라이브러리의 일부를 캡슐화하여 단순한 인터페이스를 제공함으로써 개발자가 시스템 전체를 쉽게 이해하고 사용할 수 있게 한다.

---

##### 구조

- Facade(퍼사드) : 시스템의 간략화된 인터페이스를 제공하는 객체
- Subsystem : Facade가 제공하는 인터페이스를 구현하는 객체의 집합

---

#### 장점과 단점

##### 장점

- 복잡한 시스템을 단순하게 사용할 수 있다.
- 시스템의 구현 내용을 숨길 수 잇어 안정성이 높아진다.
- 시스템의 각 부분이 독립적으로 개발될 수 있어 유연성이 높아진다.
- 유지보수가 쉬워진다.

##### 단점

- 퍼사드 객체가 추가되면서 시스템이 복잡해질 수 있다.
- 퍼사드 객체가 제공하는 인터페이스가 제한적이기 때문에 일부 기능을 사용하지 못할 수 있다.

---

#### 자바스크립트 퍼사드 패턴

- 객체의 복잡한 인터페이스를 단순한 인터페이스로 래핑

객체 간의 상호작용을 간소화하고 클라이언트 코드와 객체 간의 결합도를 낮추는데 유용하다.

객체를 생성하는 함수를 노출시켜 클라이언트 코드에서 직접 객체를 생성하는 대신 팩토리 함수를 사용한다.

- 팩토리 함수는 내부적으로 복잡한 객체를 생성하고 해당 객체를 단순한 인터페이스로 래핑한 후 반환한다.
- 클라이언트 코드는 이러한 단순한 인터페이스만 사용하여 객체와 상호작용 할 수 있다.

```js
function createRequest(method, url, data) {
  var xhr = new XMLHttpRequest();
  xhr.open(method, url);
  xhr.setRequestHeader('Content-Type', 'application/json');
  xhr.onreadystatechange = function () {
    if (xhr.readyState === XMLHttpRequest.DONE) {
      console.log(xhr.responseText);
    }
  };
  xhr.send(JSON.stringify(data));
}
```

`createRequest` 함수는 XMLHttpRequest 객체를 생성하고, 이 객체를 단순한 인터페이스로 래핑

- 클라이언트 코드에서는 이 함수를 사용하여 HTTP 요청을 보내기만 하면 된다.
- 클라이언트 코드와 XMLHttpRequest 객체 간의 결합도가 낮아지고, XMLHttpRequest 객체의 내부 구현 변경에도 영향을 받지 않는 안정적인 코드를 작성할 수 있다.

```js
createRequest('POST', '/api/users', { name: 'John', age: 30 });
```

##### 예제 2

```js
// 퍼사드 객체 생성
var Module = {
  add: function (x, y) {
    return x + y;
  },
  subtract: function (x, y) {
    return x - y;
  },
  multiply: function (x, y) {
    return x * y;
  },
  divide: function (x, y) {
    return x / y;
  },
};

// 퍼사드를 사용한 계산기 모듈
var Calculator = {
  calculate: function (operation, x, y) {
    var result;
    switch (operation) {
      case 'add':
        result = Module.add(x, y);
        break;
      case 'subtract':
        result = Module.subtract(x, y);
        break;
      case 'multiply':
        result = Module.multiply(x, y);
        break;
      case 'divide':
        result = Module.divide(x, y);
        break;
      default:
        throw new Error('Invalid operation: ' + operation);
    }
    return result;
  },
};

// 예시
var result = Calculator.calculate('add', 1, 2);
console.log(result); // 3

var result = Calculator.calculate('multiply', 3, 4);
console.log(result); // 12
```

`Module` 객체를 통해 덧셈, 뺄셈, 곱셈, 나눗셈 기능을 구현하고, `Calculator` 객체를 통해 이를 통합하여 사용하고 있다.

`Calculator` 객체는 `calculate` 메서드를 통해 `Module` 객체의 기능을 사용하고, 이를 호출하는 클라이언트 코드는 `Calculator` 객체만 알면 된다.

---

###### 추가 실사용 예제

1. JQuery 라이브러리

- DOM 조작과 이벤트 핸들링 등을 쉽게 하기 위한 라이브러리
- 내부에서 퍼사드 패턴을 사용하여 구현된다.

2. 구글 맵 API

- 구글 맵 API에서는 맵의 생성, 마커 추가, 이벤트 핸들링 등을 위한 메서드

3. 파일 업로드 라이브러리

- 파일 업로드 라이브러리는 파일 선택, 업로드 진행 상황 표시 등을 위한 메서드
