## Chapter 9

### Composite Pattern

객체들의 관계를 트리 구조로 구성하여 부분-전체 계층을 표현하는 패턴

- 개별 객체와 복합객체를 모두 같은 방식으로 다룰 수 있으며, 일관성 있는 인터페이스를 제공한다.
- 클라이언트 코드는 복합 객체와 개별 객체를 신경쓰지 않고 일관된 방식으로 다룰 수 있으며, 객체 간의 관계를 트리 구조로 표현할 수 있다.
- 복합객체의 내부 구조를 변경해도 클라이언트 코드에 영향을 미치지 않으므로 유연성이 향상된다.

#### 예시

자바스크립트에서는 DOM 트리가 컴포지트 패턴의 좋은 예시
DOM트리는 요소 노드(Element Node)와 텍스트 노드(Text Node)를 가지며, 요소 노드는 다시 자식 요소를 가질 수 있다.

- 러한 구조를 통해 HTML 문서를 표현하며, JavaScript로 DOM을 조작할 수 있다.

---

#### 구성 요소

1. Component[^component]: 복합 객체와 개별 객체 모두가 구현해야 하는 인터페이스를 정의

- 이 인터페이스는 복합 객체에서는 자식 요소를 추가하거나 제거하는 기능등을 포함할 수 있다.

2. Leaf: 개별 객체를 나타낸다.

- 이 객체는 Component 인터페이스를 구현하며, 자식 요소를 가질 수 없다.

3. Composite: 복합 객체를 나타낸다.

- 이 객체는 Component 인터페이스를 구현하며, 자식 요소를 추가하거나 제거할 수 있다.
- 복합 객체는 자식 요소로 Leaf나 다른 Composite 객체를 가질 수 있다.

---

#### 개별 객체와 복합 객체?

- 개별 객체(Leaf)는 단일 객체를 나타낸다.
- 복합 객체(Composite)는 여러 개별 객체와 또 다른 복합 객체를 포함하는 객체를 나타낸다.

복합 객체는 개별 객체와 같은 방식으로 작동할 수 있지만, 더 큰 객체이기 때문에 복잡한 계층 구조를 가지고 있다.

- 이 계층 구조를 통해 복합 객체는 자식 요소를 포함할 수 있으며, 이러한 자식 요소는 다시 개별 객체이거나 다른 복합 객체일 수 있다.

개별 객체와 복합 객체는 같은 인터페이스를 구현하며, 클라이언트 코드에서는 이 두 객체를 구분하지 않고 동일한 방식으로 사용할 수 있다.

---

#### 장점과 단점

##### 장점

- 일관성 있는 인터페이스를 제공함으로써 개별 객체와 복합 객체를 동일한 방식으로 다룰 수 있어 코드 유지 보수가 쉬워진다.
- 복잡한 계층 구조를 효율적으로 관리할 수 있어서 객체 간의 관계를 트리 구조로 표현할 수 있다.
- 복합 객체를 사용하여 다양한 형태의 객체를 쉽게 생성할 수 있다.
- 객체들의 관계를 깊이(depth)와 크기(size)로 정의할 수 있으므로 유연성이 높다.

##### 단점

- 복합 객체를 생성할 때 일부 클라이언트 코드에서는 개별 객체와 복합 객체를 구분하여 처리해야 할 수 있다.
- 일부 구현에서는 객체의 구조가 고정되어 있어서 동적인 변경이 어려울 수 있다.
- 복합 객체 내의 개별 객체들이 많아질수록 복잡성이 증가하여 처리 시간이 늘어날 수 있다.
- 복합 객체를 구성하는 개별 객체와 복합 객체를 다루는 데에는 불필요한 오버헤드[^overhead]가 발생할 수 있다.

---

#### 예제 1

예제: 파일 탐색기
파일 탐색기는 많은 파일과 폴더를 가지는 디렉토리를 탐색하는데 사용된다. 이를 컴포지트 패턴으로 구현할 수 있다.

```js
// 파일과 폴더를 나타내는 클래스
class File {
  constructor(name) {
    this.name = name;
  }
  display() {
    console.log(`File: ${this.name}`);
  }
}

class Folder {
  constructor(name) {
    this.name = name;
    this.children = [];
  }
  add(child) {
    this.children.push(child);
  }
  remove(child) {
    const index = this.children.indexOf(child);
    if (index !== -1) {
      this.children.splice(index, 1);
    }
  }
  getChild(index) {
    return this.children[index];
  }
  display() {
    console.log(`Folder: ${this.name}`);
    for (const child of this.children) {
      child.display();
    }
  }
}
```

`File 클래스` : 파일
`Folder 클래스` : 폴더

- `add 메소드` : 자식을 추가
- `remove 메소드` : 자식을 제거
- `getChild 메소드` : 인덱스에 해당하는 자식을 반환
- `display 메소드` : 폴더와 폴더에 속한 자식을 출력

```js
// 파일 탐색기
const root = new Folder('root');
const folder1 = new Folder('folder1');
const folder2 = new Folder('folder2');
const file1 = new File('file1');
const file2 = new File('file2');
const file3 = new File('file3');

root.add(folder1);
root.add(folder2);
folder1.add(file1);
folder2.add(file2);
folder2.add(file3);

root.display();
```

1. `root` 폴더 생성
2. `folder1`과 `folder2` 폴더를 `root` ㄹ폴더에 추가
3. `folder1`에 `file1` 추가
4. `folder2`에 `file2`, `file3` 파일을 추가
5. `root` 폴더 출력

```makefile
Folder: root
Folder: folder1
File: file1
Folder: folder2
```

---

#### 예제 2

직원 관리 시스템

- 직원은 `Employee` 클래스로 구현
- `Manger` 클래스와 `TeamLead` 클래스는 `Employee` 클래스를 상속 하여 만들 수 있다.

- `Manager` 클래스는 여러 명의 `Employee`나 `TeamLead`를 관리하며, `TeamLead` 클래스는 여러 명의 `Employee`를 관리한다.

- `Manager`와 `TeamLead` 클래스는 `add`, `remove`, `getSubordinates` 메소드를 가지고 있다.

```js
class Employee {
  constructor(name, salary) {
    this.name = name;
    this.salary = salary;
  }
  getName() {
    return this.name;
  }
  getSalary() {
    return this.salary;
  }
  setName(name) {
    this.name = name;
  }
  setSalary(salary) {
    this.salary = salary;
  }
  getRoles() {
    return this.roles;
  }
  addRole(role) {
    this.roles.push(role);
  }
}

class Manager extends Employee {
  constructor(name, salary) {
    super(name, salary);
    this.subordinates = [];
  }
  add(employee) {
    this.subordinates.push(employee);
  }
  remove(employee) {
    const index = this.subordinates.indexOf(employee);
    if (index !== -1) {
      this.subordinates.splice(index, 1);
    }
  }
  getSubordinates() {
    return this.subordinates;
  }
}

class TeamLead extends Employee {
  constructor(name, salary) {
    super(name, salary);
    this.subordinates = [];
  }
  add(employee) {
    this.subordinates.push(employee);
  }
  remove(employee) {
    const index = this.subordinates.indexOf(employee);
    if (index !== -1) {
      this.subordinates.splice(index, 1);
    }
  }
  getSubordinates() {
    return this.subordinates;
  }
}
```

```js
const john = new Employee('John Doe', 10000);
const jane = new Employee('Jane Doe', 9000);
const mary = new Employee('Mary Jane', 8000);

const teamLead1 = new TeamLead('Peter Parker', 12000);
const teamLead2 = new TeamLead('Harry Osborn', 11000);

const manager = new Manager('Nick Fury', 15000);

teamLead1.add(john);
teamLead1.add(jane);
teamLead2.add(mary);

manager.add(teamLead1);
manager.add(teamLead2);

console.log(`Manager: ${manager.getName()}, Salary: ${manager.getSalary()}`);
for (const teamLead of manager.getSubordinates()) {
  console.log(
    `- Team Lead: ${teamLead.getName()}, Salary: ${teamLead.getSalary()}`,
  );
  for (const employee of teamLead.getSubordinates()) {
    console.log(
      `-- Employee: ${employee.getName()}, Salary: ${employee.getSalary()}`,
    );
  }
}
```

`Employee`클래스를 사용하여 세명의 직원을 생성하고,
`TeamLead`클래스를 사용하여 두명의 팀장을 생성한다.
또한 `Manager`클래스를 사용하여 한명의 매니저를 추가 생성한다.

`teamLead1`은 `john`과 `jane`을, `teamLead2`에는 `mary`를 추가한다. 그리고 `manager`는 `teamLead1`과 `teamLead2`를 추가한다.

`console.log`문을 사용하여 각 직원의 이름과 급여를 출력한다.
`manager`의 하위 직원들을 출력하기 위해서는 `manager.getSubordinates()` 메소드를 사용하여 하위 팀장을 가져온 다음, 각 팀장의 하위 직원들을 출력한다.

```yaml
Manager: Nick Fury, Salary: 15000
- Team Lead: Peter Parker, Salary: 12000
-- Employee: John Doe, Salary: 10000
-- Employee: Jane Doe, Salary: 9000
- Team Lead: Harry Osborn, Salary: 11000
-- Employee: Mary Jane, Salary: 8000
```

[^component]: 컴포넌트(Component)는 소프트웨어에서 재사용 가능한 모듈화된 부분, 독립적으로 동작할 수 있으며, 다른 컴포넌트와 결합하여 더 큰 기능을 제공하는 소프트웨어 구성요소 즉, 단일 기능이나 특정 역할을 수행하는 모듈
[^overhead]: 특정한 기능을 수행하기 위해 추가로 사용되는 컴퓨터 자원을 치칭하는 것
