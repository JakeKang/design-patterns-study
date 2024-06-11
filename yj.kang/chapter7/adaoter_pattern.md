## Chapter 7

### Adapter Pattern

---

한 인터페이스를 다른 인터페이스로 변환하는 패턴
호환되지 않는 인터페이스 때문에 함께 동작하지 못하는 클래스를 연결해서 함께 동작할 수 있도록 한다.

- 기존 코드를 수정하지 않고 새로운 코드를 추가하여 기능을 확장할 수 있다.

---

#### 구성 요소

1. Target Interface : 클라이언트가 사용할 인터페이스
2. Adaptee : 호환되지 않는 인터페이스를 가진 클래스
3. Adapter : Adaptee를 Target Interface와 연결해주는 클래스

- 호환성 문제를 해결 할 수 있다.
- 새로운 기능을 추가하려고 할 때 기존 코드를 수정하지 않고도 새로운 코드를 추가하여 기능을 확장할 수 있다.

---

#### 장점과 단점

##### 장점

- 호환성이 없는 클래스나 인터페이스를 연결하여 사용할 수 있다.
- 기존 코드를 변경하지 않고도 새로운 코드를 함께 사용할 수 있다.
- 코드의 재사용성이 높아진다.

##### 단점

- 어탭터 클래스가 많아지면 코드의 복잡성이 증가할 수 있다.
- 어댑터 클래스를 구현할 때, 기존 클래스나 인터페이스의 기능을 모두 구현해야 하는 경우가 있다.
- 런타임 시 어댑터 객체를 생성해야 하므로 성능 저하가 있을 수 있다.

따라서 어댑터 패턴은 호환성이 없는 코드를 함께 사용해야 하는 상황에서 유용하게 사용될 수 있으나, 필요하지 않은 경우에는 코드의 복잡성을 증가시키므로 사용하지 않는 것이 좋다.

---

#### 자바스크립트 어댑터 패턴

- 일반적으로 객체를 다른 인터페이스로 래핑하여 기존의 코드와 호환되도록 만드는 방법으로 구현

```js
// 기존의 코드
class LegacyRectangle {
  constructor(x, y, w, h) {
    this.x1 = x;
    this.y1 = y;
    this.x2 = x + w;
    this.y2 = y + h;
  }

  display() {
    console.log(
      `Legacy rectangle with points: (${this.x1},${this.y1}) and (${this.x2},${this.y2})`,
    );
  }
}

// 새로운 인터페이스
class Rectangle {
  constructor(x1, y1, x2, y2) {
    this.x1 = x1;
    this.y1 = y1;
    this.x2 = x2;
    this.y2 = y2;
  }

  display() {
    console.log(
      `Rectangle with points: (${this.x1},${this.y1}) and (${this.x2},${this.y2})`,
    );
  }
}

// 어댑터
class LegacyRectangleAdapter extends Rectangle {
  constructor(x, y, w, h) {
    super(x, y, x + w, y + h);
    this.legacyRectangle = new LegacyRectangle(x, y, w, h);
  }

  display() {
    this.legacyRectangle.display();
  }
}

// 기존의 코드 사용
let r = new Rectangle(10, 10, 20, 20);
r.display();

// 어댑터를 사용하여 기존의 코드 호환
let lr = new LegacyRectangleAdapter(10, 10, 20, 20);
lr.display();
```

- `LegacyRectangle` 클래스 : 기존의 코드
- `Rectangle` 클래스 : 새로운 인터페이스
- `LegacyRectangleAdapter` 클래스 : `Reactangle` 클래스의 서브클래스로 `LegacyRectangle` 클래스를 래핑하여 `Rectangle` 클래스와 호환되도록 만들어진 어댑터

`r.display()`는 새로운 인터페이스를 사용하여 `Rectangle` 클래스의 `display()` 메서드를 호출한다.

반면 `lr.display()`는 `LegacyRectangleAdapter` 클래스의 `display()` 메서드를 호출하고,
이 메서드 내부에서 `LegacyRectangle` 클래스의 `display()` 메서드를 호출한다.

이를 통해 기존의 코드를 새로운 인터페이스와 호환되도록 사용할 수 있다.

---

#### 예제 2

- 고전적인 게임기에 삽입되는 게임 카트리지를 컴퓨터에서 실행할 수 있는 형태로 변환하는 어댑터

###### 조건

- 기존의 카트리지를 읽는 클래스로 인터페이스를 구현
- 게임 카트리지를 컴퓨터에서 실행 가능한 형태로 변환하는 클래스 구현
  - 이 클래스는 인터페이스를 구현하고, 게임 카트리지를 읽는 클래스를 멤버 변수로 가지고 있다.

```js
// 게임 카트리지 읽기 인터페이스
class GameCartridgeReader {
  readCartridge() {}
}

// 고전적인 게임기 카트리지 읽기 클래스
class GameCartridgeReaderOld {
  readCartridge() {
    console.log('Reading game cartridge...');
  }
}

// 게임 카트리지를 컴퓨터에서 실행 가능한 형태로 변환하는 어댑터 클래스
class GameCartridgeAdapter {
  constructor(gameCartridgeReader) {
    this.gameCartridgeReader = gameCartridgeReader;
  }

  readCartridge() {
    console.log('Converting game cartridge to computer-executable code...');
    this.gameCartridgeReader.readCartridge();
  }
}

// 사용 예시
const gameCartridgeReaderOld = new GameCartridgeReaderOld();
const gameCartridgeAdapter = new GameCartridgeAdapter(gameCartridgeReaderOld);
gameCartridgeAdapter.readCartridge();
```

- `GameCartridgeReader` 인터페이스를 구현한 `GameCartridgeReaderOld` 클래스는 게임 카트리지를 읽는 클래스
  - 게임 카트리지를 읽는 기능을 구현하고 잇다.

1. 게임 카트리지를 컴퓨터에서 실행 가능한 형태로 변환하는 `GameCartridgeAdapter` 클래스를 구현

   - `GameCartridgeReader` 인터페이스를 구현하고, `GameCartridgeReaderOld` 클래스를 멤버 변수로 가지고 있다.

2. `readCartridge()` 메소드에서는 게임 카트리지를 컴퓨터에서 실행 가능한 형태로 변환하는 작업을 수행하고, `GameCartridgeReaderOld` 클래스의 `readCartridge()` 메소드를 호출

3. 이렇게 구현된 `GameCartridgeAdapter` 클래스를 통해, 기존의 `GameCartridgeReaderOld` 클래스가 제공하는 기능을 사용

---

###### 추가 실사용 예제 추천

1. 다양한 데이터소스에 대한 접근

- 어떤 애플리케이션에서는 여러 데이터소스(예: API, 데이터베이스 등)에 대한 접근이 필요합니다. 이때, 데이터소스의 형식이나 인터페이스가 다르다면, 이를 일관된 인터페이스로 제공하는 어댑터 패턴을 사용할 수 있다.

2. 웹 브라우저 호환성 문제

- 웹 브라우저마다 지원하는 JavaScript 기능이나 API가 다르기 때문에, 이를 보완하기 위해 어댑터 패턴을 사용할 수 있다.

3. 타사 라이브러리 호환성 문제

- 각 라이브러리의 인터페이스를 일관된 인터페이스로 제공하는 어댑터 패턴을 사용하면 각 라이브러리 간의 호환성 문제를 해결할 수 있다.

4. 기존 코드 확장

- 기존 코드를 확장할 때, 기존 코드가 제공하는 인터페이스와 확장하려는 기능이 요구하는 인터페이스가 다를 경우, 이를 일관된 인터페이스로 제공하는 어댑터 패턴을 사용할 수 있다.

---

###### 웹 브라우저 호환성 문제 예제

```js
// XMLHttpRequest 객체 생성
function createXHR() {
  if (typeof XMLHttpRequest !== 'undefined') {
    return new XMLHttpRequest();
  } else if (typeof ActiveXObject !== 'undefined') {
    if (typeof arguments.callee.activeXString !== 'string') {
      var versions = [
          'MSXML2.XMLHttp.6.0',
          'MSXML2.XMLHttp.3.0',
          'MSXML2.XMLHttp',
        ],
        i,
        len;
      for (i = 0, len = versions.length; i < len; i++) {
        try {
          new ActiveXObject(versions[i]);
          arguments.callee.activeXString = versions[i];
          break;
        } catch (ex) {
          // continue
        }
      }
    }
    return new ActiveXObject(arguments.callee.activeXString);
  } else {
    throw new Error('No XHR object available.');
  }
}

// 예시
var xhr = createXHR();
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    if ((xhr.status >= 200 && xhr.status < 300) || xhr.status === 304) {
      console.log(xhr.responseText);
    } else {
      console.log('Request was unsuccessful: ' + xhr.status);
    }
  }
};
xhr.open('get', 'example.php', true);
xhr.send(null);
```

XMLHttpRequest 객체를 생성할 때 브라우저 호환성을 고려하여 ActiveXObject를 사용하도록 구현하였습니다. 이렇게 하면 IE와 같은 구형 브라우저에서도 정상적으로 XMLHttpRequest 객체를 생성할 수 있다.
