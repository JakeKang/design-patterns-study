## Chapter 6

### Command Pattern

실행될 기능을 객체로 캡슐화하여 필요한 시점에서 호출하거나 저장해두었다가 실행할 수 있도록 하는 패턴

- 실행될 기능의 이력을 관리하고, 취소, 재실행, 로깅 등의 작업을 수행할 수 있다.

기능을 수행하는 객체와 그 기능을 호출하는 객체(Invoker) 를 분리한다.

- 기능의 변경이나 추가에 대해 유연하게 대처할 수 있다.
- 기능의 호출 정보를 객체화하기 때문에, 데이터베이스나 파일에 저장햐여 기능의 이력을 관리하거나 복구하는 등의 작업을 수행할 수 있다.

---

### 주요 구성요소

1. 커맨더(Command) 객체 : 실행될 기능을 캡슐화한 객체
   - 실행될 기능은 메서드로 구현되며, 호출하면 기능이 수행된다.
2. 인보커(Invoker) 객체 : 커맨드 객체를 보관하고, 커맨드 객체를 실행하는 객체
   - 커맨드 객체를 실행하기 전에 해당 커맨드 객체를 저장하거나 취소할 수 있다.
3. 리시버(Receiver) 객체 : 커맨드 객체가 호출하는 메서드를 가지고 있는 객체
   - 리시버 객체는 커맨드드 객체에서 호출되는 메서드를 구현
4. 클라이언트(Client) 객체 : 인보커 객체와 리시버 객체를 생성하고, 커맨드 객체를 인보커에게 전달하는 객체

이러한 구성요소들이 상호작용하여 커맨드 패턴이 동작하게 된다.

클라이언트 객체는 필요한 기능을 수행하는 커맨드 객체를 생성한다. 이 때, 커맨드 객체는 자신이 수행할 기능과, 그 기능을 수행하는 리시버 객체를 가지고 있다.

클라이언트 객체는 생성한 커맨드 객체를 인보커 객체에게 전달하고 인보커 객체는 전달받은 커맨드 객체를 저장하거나, 필요한 시점에서 execute 메서드를 호출하여 기능을 수행한다.

---

#### 사용할 수 있는 상황

1. 실행 취소(Undo) 기능

   - 애플리케이션에서는 사용자의 작업을 커맨드 객체로 캡슐화하여 인보커 객체에 저장한다. 사용자가 실행 취소 기능을 호출하면 인보커 객체는 저장된 커맨드 객체를 꺼내어 execute 메서드를 실행하여 이전 상태로 되돌린다.

2. 비동기 처리

   - Javascript에서는 Promise나 async/await와 같은 비동기 처리를 위한 문법의 비동기처리를 위한 함수를 커맨드 객체로 캡슐화하고 인보커 객체에 저장함으로써 비동기 처리를 제어할 수 있다.

3. 키보드, 마우스 이벤트 처리

   - 이벤트 객체를 커맨드 객체로 캡슐화하여 인보커 객체에 저장할 수 있다. 저장된 커맨드 객체는 나중에 다시 실행될 수 있다.

4. 트랜잭션 처리
   - 데이터베이스 등에서 트랜잭션 처리시, 수행된 쿼리를 커맨드 객체로 캡슐화하여 인보커 객체에 저장한다.
   - 트랜잭션 처리를 취소해야 하는 경우 인보커 객체는 저장된 커맨드 객체를 꺼내어 execute 메서드를 실행하여 트랜잭션을 롤백한다.

---

### 장점과 단점

#### 장점

1. 유연성 : 객체를 인수로 받기 때문에 어떤 개겣든 처리할 수 있다. 실행 중에 실행할 작업을 쉽게 바꿀 수 있다.
2. 캡슐화 : 객체의 상태를 캡슐화하여 객체 간의 결합도를 줄인다. 객체 간의 의존성을 최소화하여 유지 보수성과 확장성을 향상 시킨다.
3. 쉬운 테스트 : 메서드가 간단한 인터페이스만을 가지고 있기 때문에 테스트하기 쉽다.
4. 기록과 취소 : 기록과 취소화 같은 기능을 쉽게 추가할 수 있다.

#### 단점

1. 객체 증가 : 명령에 대한 클래스를 만들어야 하므로 객체의 수가 늘어날 수 있다.
2. 처리 시간 : 추가적인 호출을 통해 명령을 처리하기 때문에 처리 시간이 늘어날 수 있다.
3. 복잡성 : 많은 객체와 클래스가 생성되기 때문에 코드가 복잡해 질 수 있다. 적절한 사용이 필요!
4. 가독성 : 커맨드 패턴을 처음 본 개발자들은 이해하기 어려울 수 있다. 이는 코드 가독성에 영향을 줄 수 있다.

---

#### 간단한 패턴 예제

```js
// Receiver 객체
class Calculator {
  constructor() {
    this.currentValue = 0;
  }

  add(value) {
    this.currentValue += value;
    console.log(`Added ${value}, currentValue is now ${this.currentValue}`);
  }

  subtract(value) {
    this.currentValue -= value;
    console.log(
      `Subtracted ${value}, currentValue is now ${this.currentValue}`,
    );
  }
}

// Command 객체
class CalculatorCommand {
  constructor(calculator, operator, value) {
    this.calculator = calculator;
    this.operator = operator;
    this.value = value;
  }

  execute() {
    this.calculator[this.operator](this.value);
  }

  undo() {
    this.calculator[this.operator](-this.value);
  }
}

// Invoker 객체
class CommandManager {
  constructor() {
    this.commands = [];
  }

  execute(command) {
    this.commands.push(command);
    command.execute();
  }

  undo() {
    const command = this.commands.pop();
    command.undo();
  }
}

// 사용 예제
const calculator = new Calculator();
const commandManager = new CommandManager();

commandManager.execute(new CalculatorCommand(calculator, 'add', 10)); // Added 10, currentValue is now 10
commandManager.execute(new CalculatorCommand(calculator, 'subtract', 5)); // Subtracted 5, currentValue is now 5

commandManager.undo(); // Added -5, currentValue is now 10
commandManager.undo(); // Added -10, currentValue is now 0
```

- Calculator 클래스가 Receiver 역할을 수행

- CalculatorCommand 클래스가 Command 역할을 수행

  - Calculator 객체와 연산자, 값을 받아서 execute() 메서드에서 해당 연산자를 실행
  - undo() 메서드에서는 해당 연산자를 실행한 결과를 되돌린다.

- CommandManager 클래스는 Invoker 역할을 수행

  - execute() 메서드를 호출하면 받은 Command 객체를 저장하고 해당 메서드를 실행한다.
  - undo() 메서드를 호출하면 저장된 Command 객체를 꺼내어 해당 메서드를 실행한다.

- CommandManager 객체가 저장한 Command 객체를 꺼내어 undo() 메서드를 실행하면, Calculator 객체의 상태를 이전 상태로 되돌릴 수 있습니다.

---

#### 그림판 애플리케이션 예제

```js
class DrawingBoard {
  constructor() {
    this.shapes = [];
  }

  addShape(shape) {
    this.shapes.push(shape);
    console.log(`Added a shape: ${shape.type}`);
  }

  removeShape(shape) {
    const index = this.shapes.indexOf(shape);
    if (index !== -1) {
      this.shapes.splice(index, 1);
      console.log(`Removed a shape: ${shape.type}`);
    }
  }

  moveShape(shape, x, y) {
    shape.move(x, y);
    console.log(`Moved a shape to (${x}, ${y})`);
  }

  resizeShape(shape, width, height) {
    shape.resize(width, height);
    console.log(`Resized a shape to (${width}, ${height})`);
  }
}

class Shape {
  constructor(type, x, y, width, height) {
    this.type = type;
    this.x = x;
    this.y = y;
    this.width = width;
    this.height = height;
  }

  move(x, y) {
    this.x = x;
    this.y = y;
  }

  resize(width, height) {
    this.width = width;
    this.height = height;
  }
}

class AddShapeCommand {
  constructor(drawingBoard, shape) {
    this.drawingBoard = drawingBoard;
    this.shape = shape;
  }

  execute() {
    this.drawingBoard.addShape(this.shape);
  }

  undo() {
    this.drawingBoard.removeShape(this.shape);
  }
}

class MoveShapeCommand {
  constructor(drawingBoard, shape, x, y) {
    this.drawingBoard = drawingBoard;
    this.shape = shape;
    this.prevX = shape.x;
    this.prevY = shape.y;
    this.x = x;
    this.y = y;
  }

  execute() {
    this.drawingBoard.moveShape(this.shape, this.x, this.y);
  }

  undo() {
    this.drawingBoard.moveShape(this.shape, this.prevX, this.prevY);
  }
}

class ResizeShapeCommand {
  constructor(drawingBoard, shape, width, height) {
    this.drawingBoard = drawingBoard;
    this.shape = shape;
    this.prevWidth = shape.width;
    this.prevHeight = shape.height;
    this.width = width;
    this.height = height;
  }

  execute() {
    this.drawingBoard.resizeShape(this.shape, this.width, this.height);
  }

  undo() {
    this.drawingBoard.resizeShape(this.shape, this.prevWidth, this.prevHeight);
  }
}

class CommandManager {
  constructor() {
    this.commands = [];
  }

  execute(command) {
    this.commands.push(command);
    command.execute();
  }

  undo() {
    const command = this.commands.pop();
    if (command) {
      command.undo();
    }
  }
}

// 사용 예제
```

커맨드 패턴은 주로 웹 애플리케이션에서 사용된다.
예를 들어, 사용자가 버튼을 클릭하면 해당 버튼에 대한 명령(Command)을 생성하고 이를 큐(Queue)에 넣습니다. 그리고 큐에서 이 명령들을 순차적으로 처리하여 특정 작업을 수행

- 자바스크립트의 DOM(Document Object Model)에서 이벤트 핸들러(Event Handler) 함수를 구현할 때 커맨드 패턴을 적용할 수 있다.
  - 이벤트 발생 시 해당 이벤트를 처리하는 함수를 정의하여 이를 명령(Command)으로 간주하고, 이벤트 발생 시에 명령(Command)을 실행하여 특정 동작을 수행한다.
