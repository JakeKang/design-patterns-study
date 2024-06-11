## Chapter 5

### Singleton Pattern

어떤 클래스의 인스턴스가 오직 하나만 존재하도록 보장하는 패턴,
전역 변수를 사용하는 것과 유사하지만, 객체 지향적으로 구현된 것이다.

- 즉, 객체를 딱 한번만 생성하고, 그 객체를 전역에서 접근할 수 있는 방법을 제공하는 디자인 패턴, 전역 객체를 생성하는 용도로도 사용된다.

- 생성자가 여러번 호출되도, 실제로 생성되는 객체는 하나이며 최초로 생성된 이후에 호출된 생성자는 이미 생성한 객체를 반환시키도록 만드는 것

#### 사용하는 이유?

객체를 생성할 때마다 메모리 영역을 할당받아야 한다. 한번의 `new` 생성자를 통해 객체를 생성한다면 메모리 낭비를 사전에 방지할 수 있다.

#### 특징

1. 오직 한 개의 객체만 존재한다.
2. 언제나 동일한 객체 인스턴스를 반환한다.
3. 전역에서 쉽게 접근할 수 있다.
4. 객체 생성 및 관리에 대한 책임을 담당하는 코드가 한곳에 집중된다.

#### 장점

1. 메모리를 효율적으로 사용할 수 있다. 여러 개의 객체 인스턴스를 생성하는 대신, 단 하나의 객체 인스턴스를 공유함으로써 메모리 사용을 최소화할 수 있다.
2. 전역에서 쉽게 접근할 수 있기 때문에, 객체의 상태를 전역적으로 관리할 수 있다.
3. 객체 생성 및 관리에 대한 책임이 한곳에 집중되기 때문에, 코드 유지보수 및 개발에 용이하다.

#### 단점

1. 객체의 상태가 전역적으로 관리되기 때문에, 다른 모듈에서 객체의 상태를 변경될 수 있다.
2. 동일한 객체 인스턴스를 공유하기 때문에 멀티스레드 환경에서는 동기화 문제가 발생할 수 있다. 여러 스레드에서 동시에 객체를 변경하려고 할 경우 동기화 문제가 발생할 수 있다.

#### Javascript 에서의 싱글턴 패턴

클래스를 사용하여 객체를 생성하는 것 외에도 함수를 사용하여 객체를 생성할 수 있기 때문에, 구현하는 방법이 조금 다를 수 있다.

- 주로 모듈 패턴이나 즉시 실행 함수로 구현된다.

###### Logger 클래스를 함수로 구현한 예제

```js
let logger = (function () {
  let instance;

  function createLogger() {
    let logs = [];

    function log(message) {
      logs.push(message);
      console.log(`[logger] ${message}`);
    }

    function printLogs() {
      console.log(`[Logger] Logs:`);
      logs.forEach((log) => console.log(log));
    }

    return {
      log: log,
      printLogs: printLogs,
    };
  }

  return {
    getInstance: function () {
      if (!instance) {
        instance = createLogger();
      }

      return instance;
    },
  };
})();

const logger1 = logger.getInstance();
const logger2 = logger.getInstance();

console.log(logger1 === logger2); // true

logger1.log('Hello');
logger2.log('world');

logger1.printLogs();
```

createLogger 함수는 Logger 클래스의 생성자 함수 역할을 하며, log 메서드와 printLogs 메서드를 구현해서 메서드들을 객체로 반환한다.

반환된 객체는 logger 변수에 할당되는데, 이 변수는 클로저(Closure)를 이용하여 비공개(private) 변수인 instance를 참조한다.

getInstance 메서드는 이전에 생성된 객체가 존재하는지 검사하고, 객체가 없는 경우에만 createLogger 함수를 호출하여 객체를 생성하고, instance 변수에 할당한다.

끝으로 생성된 인스턴스를 반환한다.

클라이언트 코드에서는 logger.getInstance() 메서드를 호출하여 유일한 객체를 가져와서 사용한다.

이렇게 하면 Logger 클래스의 인스턴스는 오직 하나만 존재하게 되므로, 어떤 클라이언트 코드에서도 동일한 인스턴스를 사용하게 된다.

###### 또 따른 예제

1. 전역 객체

- 전역 객체는 자바스크립트에서 하나의 객체만 존재한다. 따라서 이 객체는 싱글턴 패턴을 따르고 있다.

```js
const Global = (function () {
  let instance;

  function createGlobal() {
    // 전역 객체를 구현한 코드
  }

  return {
    getInstance: function () {
      if (!instance) {
        instance = createGlobal();
      }

      return instance;
    },
  };
})();

const globalObj1 = Global.getInstance();
const globalObj2 = Global.getInstance();

console.log(globalObj1 === globalObj2); // true
```

2. Config 객체

- 웹 애플리케이션에서는 Config 객체를 사용하여 환경 설정 정보를 저장하는 경우가 많다.

```js
const Config = (function () {
  let instance;

  function createConfig() {
    const config = {
      // 설정 정보를 저장한 객체
    };

    return {
      getConfig: function (key) {
        return config[key];
      },
      setConfig: function (key, value) {
        config[key] = value;
      },
    };
  }

  return {
    getInstance: function () {
      if (!instance) {
        instance = createConfig();
      }

      return instance;
    },
  };
})();

const config1 = Config.getInstance();
const config2 = Config.getInstance();

config1.setConfig('language', 'en');
config2.setConfig('theme', 'dark');

console.log(config1.getConfig('language')); // 'en'
console.log(config1.getConfig('theme')); // 'dark'
console.log(config2.getConfig('language')); // 'en'
console.log(config2.getConfig('theme')); // 'dark'
```

---

###### 추가 정리

1. 전역 변수를 사용할 때

   - 전역 변수를 사용하는 것은 코드의 유지 보수성을 저해할 수 있다.
   - 전역 변수 대신 싱글톤 패턴을 사용하여 인스턴스화된 객체를 사용하면 전역 변수를 사용하지 않아도 되며, 코드 유지 보수성이 향상

2. 데이터 캐시

   - 데이터베이스에서 데이터를 가져와서 캐시하고, 이후에 다시 데이터를 가져올 때는 캐시된 데이터를 사용하는 것이 데이터베이스 연결 및 데이터베이스 작업을 줄일 수 있다.
   - 이 때, 싱글톤 패턴을 사용하여 데이터를 캐시하면 객체 인스턴스를 재사용하므로, 메모리 사용이 효율적이고 성능 향상을 기대할 수 있다.

3. 로그인 기능

   - 로그인 기능은 일반적으로 한 번에 하나의 사용자만 사용할 수 있어야 하므로, 싱글톤 패턴을 사용하여 사용자 인증 정보를 관리

4. 모듈 패턴
   - 자바스크립트 모듈 패턴은 싱글톤 패턴의 한 형태
   - 모듈 패턴은 객체 리터럴을 반환하는 함수를 정의하여, 객체 리터럴을 사용하여 모듈의 변수와 메소드를 캡슐화
   - 이렇게 구현된 모듈은 전역 네임스페이스를 오염시키지 않고 사용할 수 있으며, 필요한 경우 인스턴스화하여 여러 개의 객체를 생성할 수도 있다.
