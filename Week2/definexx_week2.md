# Section 5~9

---

## 함수 타입

---

**타입스크립트에서는 함수의 매개변수와 함수의 타입(return 값의 타입)을 정의해야함 (추론도 가능)**

```tsx
// 함수 선언식
function func(a: number, b: number) {
  return a + b;
}

// 화살표 함수
const add = (a: number, b: number) => a + b;
```

**함수의 매개변수**

```tsx
function introduce(name = "이정환", age: number, tall?: number) {
  console.log(`name: ${name}`);
  if (typeof tall === "number") {
    console.log(`tall: ${tall + 10}`);
  }
}
```

**→ 필수 매개변수는 선택적 매개변수보다 앞에 배치를 해야함**

**→ 선택적 매개변수를 사용할 때는, 값이 undefined일 경우를 위해 타입 가드를 이용해 타입을 좁힐 필요가 있음**

**Rest Parameter**

```tsx
function getSum(...rest: number[]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));

  return sum;
}

getSum(1, 2, 3);
getSum(1, 2, 3, 4, 5);
```

**→ 여러 개의 인자를 배열의 형태로 담으므로, `number[]` 와 같이 타입을 정의**

**(길이를 제한하고 싶으면 튜플 사용)**

## 함수 타입 표현식 (Function type expression)

---

**타입 별칭을 이용해 함수의 타입을 별도로 정의하는 문법**

```tsx
type Add = (a: number, b: number) => number;

const add: Add = (a, b) => a + b;
```

→ 호출 시그니처 or 함수 시그니처라고도 불림 (공식명칭 X)

## 호출 시그니쳐 (Call Signature)

---

**함수의 타입을 직접 정의할 때와 같은 구문을 떼어내 함수의 타입을 정의하는 문법**

```tsx
function func (a: number, b: number):void {}

type Operation = {
  (a: number, b: number): number;
};
```

→ 함수도 객체이므로 객체 타입을 정의하듯이 중괄호를 사용

**→ 호출 시그니쳐를 사용할 때, 객체 프로퍼티를 추가적으로 정의해주면 하이브리드 타입으로도 사용 가능**

## 함수 타입 호환성

---

특정 함수 타입을 다른 함수 타입으로 취급할 수 있는지!

1. **반환값의 타입이 호환되는가?**
2. **매개변수의 타입이 호환되는가?**

**기준 1. 반환값이 호환되는가?**

```tsx
type A = () => number;
type B = () => 10;

let a: A = () => 10;
let b: B = () => 10;

a = b;
b = a; // Type 'A' is not assignable to type 'B'. Type 'number' is not assignable to type '10'.
```

**기준 2. 매개변수가 호환되는가?**

**a. 매개변수의 개수가 같을 때**

```tsx
type C = (value: number) => void;
type D = (value: 10) => void;

let c: C = (value) => {};
let d: D = (value) => {};

c = d; // Type 'number' is not assignable to type '10'.
d = c;
```

**→ 매개변수 간의 동작이 Upcasting일 경우에는 오류 발생 / Downcasting일 경우에는 오류 발생 X**

**Example) 매개변수가 객체인 경우**

```tsx
type Animal = {
  name: string;
};

type Dog = {
  name: string;
  color: string;
};

let animalFunc = (animal: Animal) => {
  console.log(animal.name);
};

let dogFunc = (dog: Dog) => {
  console.log(dog.name);
  console.log(dog.color);
};

// 오류 발생하는 부분
animalFunc = dogFunc;  // 이 부분에서 타입 오류 발생

let testFunc = (animal: Animal) => {
  console.log(animal.name);
  console.log(animal.color);  // 여기서 'color' 속성이 Animal 타입에 없어서 오류 발생
};
```

**→ 더 구체적인 프로퍼티를 가지는 객체를 매개변수로 가지는 함수를**

**더 추상적인 객체를 매개변수로 가지는 함수에 할당할 수 없음**

**(매개변수 간의 동작이 UpCasting일 경우에 오류 발생)**

**b. 매개변수의 개수가 다를 때**

**할당하려고 하는 함수의 매개변수의 개수가 더 적을 경우에만 가능**

**→ 매개변수의 타입 자체가 다르면 호환되지 않음**

```tsx
type Func1 = (a: number, b: number) => void;
type Func2 = (a: number) => void;

let func1: Func1 = (a, b) => {};
let func2: Func2 = (a) => {};

func1 = func2;
func2 = func1;  // Func1 형식을 Func2에 할당할 수 없다는 오류 발생
```

## 함수 오버로딩

---

**함수를 매개변수의 개수나 타입에 따라 여러가지 버전으로 정의하는 방법**

**→ JavaScript에서는 지원이 안되고, TypeScript에서만 지원됨**

### 오버로드 시그니쳐 (Overload Signature)

---

**함수를 오버로딩하기 위해서 각각 매개변수의 형식마다 다른 버전을 명시하기 위해 사용**

```tsx
function func(a: number): void;
function func(a: number, b: number, c: number): void;
```

### 구현 시그니쳐

---

**어떤 함수가 오버로드 시그니쳐를 가지고 있으면 함수를 호출할 때**

**인자들의 타입이 실제 구현부에 정의된 매개변수의 개수나 타입을 따르지 않고,**

**오버로드 시그니쳐의 버전 중 하나를 따라감**

```tsx
function func(a: number): void;
function func(a: number, b: number, c: number): void;

function func() {};

func(); // Expected 1-3 arguments, but got 0.
func(1);
func(1,2); // No overload expects 2 arguments, but overloads do exist that expect either 1 or 3 arguments.
func(1,2,3);
```

**구현부의 매개변수를 정의할 때, 선택적 프로퍼티를 적절하게 사용해줘야 오버로드 시그니쳐와 호환 가능**

```tsx
function func(a: number): void;
function func(a: number, b: number, c: number): void;

function func(a: number, b?: number, c?: number) {
  if (typeof b === "number" && typeof c === "number") {
    console.log(a + b + c);
  } else {
    console.log(a * 20);
  }
}
```

→ 첫 번째 버전에는 b 와 c 매개변수가 존재하지 않으므로, 선택적 프로퍼티로 매개변수를 정의할 필요가 있음

## 사용자 정의 타입 가드 (Custom type guard)

---

**함수를 이용해 타입 가드를 커스텀해 사용할 수 있는 문법**

**→ 타입 단언에 사용하는 `as` 와 `is` 키워드 사용**

```tsx
type Dog = {
  name: string;
  isBark: boolean;
};

type Cat = {
  name: string;
  isScratch: boolean;
};

type Animal = Dog | Cat;

function isDog(animal: Animal): animal is Dog {
  return (animal as Dog).isBark !== undefined;
}

function isCat(animal: Animal): animal is Cat {
  return (animal as Cat).isScratch !== undefined;
}

function warning(animal: Animal) {
  if (isDog(animal)) {
    // 강아지
    animal;
  } else if (isCat(animal)) {
    // 고양이
    animal;
  }
}
```

## 인터페이스(Interface)

---

“상호간에 약속된 규칙”

**타입 별칭 외에 타입에 이름을 지어주는 또 다른 문법**

→ **객체의 구조를 정의하는데 특화**된 문법 (**상속과 합성의 기능** 제공)

```tsx
interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void;
  sayHi(a: number, b: number): void;
}
```

**→ `|`(Union) 이나 `&`(Intersection) 문법은 사용할 수 없음**

```tsx
interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void; // 호출 시그니쳐 사용
  sayHi(a: number, b: number): void;
}

const person: Person = {
  name: "이정환",
  sayHi: function () {
    console.log("Hi");
  },
};

person.sayHi();
person.sayHi(1, 2);
```

**→ 함수의 오버로딩을 구현하고 싶을 때는, 인터페이스 내에서 호출 시그니쳐 문법을 사용해야함**

### 인터페이스 확장

---

**`extends` 키워드를 사용해 프로퍼티들을 상속할 수 있음**

```tsx
interface Animal {
  name: string;
  color: string;
};

interface Dog extends Animal {
  isBark: boolean;
}

const dog: Dog = {
  name: "",
  color: "",
  isBark: true,
};
```

**상속을 받는 interface에서 동일한 프로퍼티의 타입을 재정의할 수 있음 (But, 원본 타입의 서브타입만 허용)**

```tsx
interface Animal {
  name: string;
  color: string;
}

interface Dog extends Animal {
  name: number; // Animal interface의 name 프로퍼티 타입이 string이므로 오류 발생
  isBark: boolean;
}

const dog: Dog = {
  name: 1,
  color: "",
  isBark: true,
};
```

**타입 별칭도 객체 형태라면 확장할 수 있음**

```tsx
type Animal = {
  name: string;
  color: string;
};

interface Dog extends Animal {
  isBark: boolean;
}
```

**다중 확장**

```tsx
interface Animal {
  name: string;
  color: string;
};

interface Dog extends Animal {
  isBark: boolean;
}

interface Cat extends Animal {
  isScratch: boolean;
}

interface Dogcat extends Dog, Cat {} // 다중 확장
```

### 인터페이스 합치기

---

**동일한 이름으로 정의된 인터페이스는 합쳐짐 (주로 모듈 보강에 사용)**

```tsx
interface Person {
  name: string;
}

interface Person {
  age: number;
}
```

**→ 오류 발생하지 않고 하나의 Person 인터페이스로 합쳐짐**

**단, 타입 충돌은 허용하지 않음**

```tsx
interface Person {
  name: string;
}

interface Person {
  name: number; // Subsequent Property declarations must have the same type.
  age: number;
}
```

**인터페이스 확장과 달리 인터페이스 선언 합침의 경우에는 서브 타입도 허용하지 않음**

```tsx
interface Person {
  name: string;
}

interface Person {
  name: "hello"; // Subsequent Property declarations must have the same type.
  age: number;
}
```

## 클래스

---

### JavaScript의 클래스

---

**클래스 선언 시에는 파스칼 표기법(e.g. Student)을 사용**

**→ 클래스는 필드, 생성자, 메소드로 구성**

**→ 클래스를 이용해 만든 객체를 인스턴스(instance)라고 함**

**→ 상속 기능 지원 (`super` 함수 활용)**

```tsx
class Student {
  // 필드
  name;
  grade;
  age;

  // 생성자
  constructor(name, grade, age) {
    this.name = name;
    this.grade = grade;
    this.age = age;
  }
  
  // 메소드
  study() {
	  console.log("열심히 공부함");
  }
  
  introduce() {
	  console.log("안녕하세요!");
  }
}

let student = new Student("김철흥", "B", 25); // student는 인스턴스
```

**상속**

```tsx
class StudentDeveloper extends Student {
  // 필드
  favoriteSkill;

  // 생성자
  constructor(name, grade, age, favoriteSkill) {
    super(name, grade, age); // 부모 클래스의 constructor 함수 호출 -> 매개변수에 값 할당됨
    this.favoriteSkill = favoriteSkill;
  }

  // 메서드
  programming() {
    console.log(`${this.favoriteSkill}로 프로그래밍 함`);
  }
}

const studentDeveloper = new StudentDeveloper("이정환", "B+", 27, "TypeScript");
console.log(studentDeveloper);
studentDeveloper.programming();
```

### TypeScript의 클래스

---

**JavaScript와는 다르게 필드와 매개변수에 타입을 정의해야함**

```tsx
class Employee {
  // 필드
  name: string;
  age: number;
  position: string;

  // 생성자
  constructor(name: string, age: number, position: string) {
    this.name = name;
    this.age = age;
    this.position = position;
  }

  // 메서드
  work() {
    console.log("일함");
  }
}
```

**타입스크립트의 클래스는 실제 타입으로도 활용할 수 있음 (타입스크립트는 구조적 타입 시스템을 따르기 때문)**

**→ 클래스에 존재하는 필드와 메소드를 갖고 있어야하는 객체 타입으로 정의가 됨**

```tsx
const employeeC: Employee = {
  name: "",
  age: 0,
  position: "",
  work() {},
};
```

**상속**

**Javascript와는 다르게 Typescript에서는 반드시 `super` 함수를 호출해야함**

```tsx
class ExecutiveOfficer extends Employee {
  // 필드
  officeNumber: number;

  // 생성자
  constructor(name: string, age: number, position: string, officeNumber: number) {
    super(name, age, position); // 반드시 호출해야함
    this.officeNumber = officeNumber;
  }
}

```

## 접근 제어자 (Access Modifier)

---

**클래스를 만들 때 특정 필드나 메소드에 접근할 수 있는 범위를 설정하는 문법 (Typescript에서만 제공되는 기능)**

**→ 기본값은 `public` 으로 자유롭게 접근 가능**

**필드에서 접근 제어자를 `private`으로 설정하면, 클래스 내 메소드에서만 사용 가능**

**→ 외부에서는 접근 불가능 (파생 클래스에서도 불가능)**

```tsx

```

**파생 클래스에서 접근 가능하게 하려면, `protected` 접근 제어자 사용**

```tsx

```

**생성자 매개변수에 접근 제어자를 표기할 경우에는 필드에 중복 선언하지 않아야함!**

```tsx
class Employee {
  // 필드 (중복 선언 X)
	name: string;
  age: number;
  position: string;

  // 생성자
  constructor(private name: string, protected age: number, public position: string) {
    this.name = name;
    this.age = age;
    this.position = position;
  }

  // 메서드
  work() {
    console.log(`${this.name} 일함`);
  }
}
```

**→ 생성자의 매개변수에 접근 제어자가 정의된 매개변수들은 필드의 값 또한 자동으로 초기화**

## 인터페이스와 클래스

---

**`implements` 키워드를 사용해 인터페이스가 정의하는 객체 타입을 클래스가 생성하도록 할 수 있음**

```tsx
interface CharacterInterface {
  name: string;
  moveSpeed: number;
  move(): void;
}

class Character implements CharacterInterface {
  name: string;
  moveSpeed: number;

  constructor(name: string, moveSpeed: number) {
    this.name = name;
    this.moveSpeed = moveSpeed;
  }

  move(): void {
    console.log(`${this.moveSpeed} 속도로 이동!`);
  }
}
```

**→ 인터페이스로 정의하는 필드들은 `public` 필드만 정의할 수 있음!**

---