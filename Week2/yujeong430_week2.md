# Section 5~9

# 5. 함수와 타입

## 함수와 타입

어떤 타입의 매개 변수를 받아서 어떤 타입의 값을 반환하는지 정의.

반환 값 타입은 자동으로 추론되므로 생략 가능.

```jsx
function func(a: number, b: number): number {
  return a + b;
}

const add = (a: number, b: number): number => a + b;
```

## 매개변수

매개변수에 기본 값이 설정되어있으면 타입 자동 추론. 

매개변수의 기본 값과 다른 타입으로 정의할 때, 기본 값과 다른 타입의 값을 인수로 전달할 때 오류 발생.

```jsx
function introduce(name = "이정환") { // 자동으로 추론.
	console.log(`name : ${name}`);
}

function introduce(name:number = "이정환") { // 오류
	console.log(`name : ${name}`);
}

function introduce(name = "이정환") {
  console.log(`name : ${name}`);
}

introduce(1); // 오류
```

### **선택적 매개변수**

매개변수 뒤에 ?를 붙이면 선택적 매개변수가 되어 생략 가능.

선택적 매개변수의 타입은 자동으로 **undefined와 유니온 된 타입**으로 추론됨.

```jsx
function introduce(name = "이정환", tall?: number) { // tall의 타입은 number | defined
  console.log(`name : ${name}`);
  if (typeof tall === "number") { // 타입 좁히기
    console.log(`tall : ${tall + 10}`);
  }
}
```

선택적 매개변수는 필수 매개변수 앞에 올 수 없음.

```jsx
function introduce(name = "이정환", tall?: number, age: number) { // 오류
 ...
}
```

### **나머지 매개변수 (rest parameter)**

```jsx
function getSum(...rest: number[]) { // number 타입의 배열
  let sum = 0;
  rest.forEach((it) => (sum += it));
  return sum;
}
```

나머지 매개변수의 길이를 고정하고 싶을 때는 튜플 타입 이용.

```jsx
function getSum(...rest: [number, number, number]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));
  return sum;
}

getSum(1, 2, 3)    // ✅
getSum(1, 2, 3, 4) // ❌
```

## 함수 타입 표현식과 호출 시그니처

### 함수 타입 표현식

함수 타입을 타입 별칭과 함께 별도로 정의하는 것.

함수 선언 및 구현 코드 /  타입 선언을 분리할 수 있어 유용.

```jsx
type Add = (a: number, b: number) => number;

const add: Add = (a, b) => a + b;

// 여러개의 함수가 동일한 타입을 갖는 경우
type Operation = (a: number, b: number) => number;

const add: Operation = (a, b) => a + b;
const sub: Operation = (a, b) => a - b;
const multiply: Operation = (a, b) => a * b;
const divide: Operation = (a, b) => a / b;
```

### 호출 시그니처

함수의 타입을 객체를 정의하듯 별도로 정의하는 것.

```jsx
type Operation2 = {
  (a: number, b: number): number;
};

const add2: Operation2 = (a, b) => a + b;
const sub2: Operation2 = (a, b) => a - b;
const multiply2: Operation2 = (a, b) => a * b;
const divide2: Operation2 = (a, b) => a / b;
```

**하이브리드 타입**

호출 시그니처 아래에 프로퍼티를 추가 정의. 함수이자 일반 객체를 의미하는 타입.

```jsx
// getCounter 함수 타입을 선언함과 동시에 프로퍼티의 타입을 선언.
interface Counter {
  (start: number): string;
  interval: number;
  reset(): void;
}

function getCounter(): Counter {
  let counter = function (start: number) {} as Counter;
  counter.interval = 123;
  counter.reset = function () {};
  return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

## 함수 타입의 호환성

특정 함수 타입을 다른 함수 타입으로 괜찮은지 아래 두 가지 기준을 바탕으로 판단하는 것.

- **두 함수의 반환 값 타입이 호환되는가?**
    
    A의 반환값 타입이 B 반환값 타입의 슈퍼타입이라면 호환 가능.
    
    ```jsx
    type A = () => number; // Number
    type B = () => 10; // Number Literal
    
    let a: A = () => 10;
    let b: B = () => 10;
    
    a = b; // ✅
    b = a; // ❌
    ```
    
- **두 함수의 매개변수 타입이 호환되는가?**
    - 매개변수의 개수가 같을 때
        
        C 매개변수의 타입이 D 매개변수 타입의 서브타입일떼 호환 가능.
        
        ```tsx
        type C = (value: number) => void;
        type D = (value: 10) => void;
        
        let c: C = (value) => {};
        let d: D = (value) => {};
        
        c = d; // ❌
        d = c; // ✅ C를 D로 취급할 수 있음.
        ```
        
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
        
        animalFunc = dogFunc; // ❌ animalFunc에는 color라는 프로퍼티가 정의되어 있지 않음.
        dogFunc = animalFunc; // ✅
        ```
        
    - 매개변수의 개수가 다를 때
        
        ```tsx
        type Func1 = (a: number, b: number) => void;
        type Func2 = (a: number) => void;
        
        let func1: Func1 = (a, b) => {};
        let func2: Func2 = (a) => {};
        
        func1 = func2; // ✅
        func2 = func1; // ❌
        ```
        

## 함수 오버로딩

하나의 함수를 매개변수의 개수나 타입에 따라 다르게 동작하도록 만드는 문법.

버전별 **오버로드 시그니쳐**를 만들어야 함.

➕ 오버로드 시그니쳐 : 구현부 없이 선언부만 만들어둔 함수.

```tsx
// 버전들 -> 오버로드 시그니쳐
function func(a: number): void; // 매개변수 한 개를 받는 버전
function func(a: number, b: number, c: number): void; // 매개변수 세 개를 받는 버전

// 실제 구현부 -> 구현 시그니쳐
function func(a: number, b?: number, c?: number) { // b, c를 선택적 매개변수로 만들어 모든 오버로드 시그니쳐와 호환되도록.
  if (typeof b === "number" && typeof c === "number") {
    console.log(a + b + c);
  } else {
    console.log(a * 20);
  }
}

func(1);        // ✅ 버전 1 - 오버로드 시그니쳐
func(1, 2);     // ❌ 
func(1, 2, 3);  // ✅ 버전 3 - 오버로드 시그니쳐
```

## 사용자 정의 타입가드

참 또는 거짓을 반환하는 함수를 이용해 타입 가드를 만들 수 있도록 도와주는 문법.

- 복잡한 타입은 typeof, instanceof 등을 활용하기 어려움
- 타입 판단 로직을 재사용하고 싶음
- in 연산자를 사용할 경우 프로퍼티의 이름이 수정되거나 추가, 삭제 될 경우 제대로 동작하지 않을 수 있음

⇒ 사용자 정의 타입가드 사용!

```tsx
type Dog = {
  name: string;
  isBark: boolean;
};

type Cat = {
  name: string;
  isScratch: boolean;
};

// Dog 타입인지 확인하는 타입 가드. Dog 타입으로 단언한 후 검사.
function isDog(animal: Animal): animal is Dog {
  return (animal as Dog).isBark !== undefined; // isBark가 있는지 없는지 확인
}

// Cat 타입인지 확인하는 타입가드
function isCat(animal: Animal): animal is Cat {
  return (animal as Cat).isScratch !== undefined;
}

function warning(animal: Animal) {
  if (isDog(animal)) { // true일 경우 Dog 타입임이 보장됨
    console.log(animal.isBark ? "짖습니다" : "안짖어요");
  } else {
    console.log(animal.isScratch ? "할큅니다" : "안할퀴어요");
  }
}
```

# 6. 인터페이스

타입에 이름을 지어주는 또 다른 문법.

```tsx
interface Person {
  name: string;
  age: number;
}

const person: Person = {
  name: "이정환",
  age : 27
};
```

**선택적 프로퍼티**

```tsx
interface Person {
  age?: number;
}
```

**읽기 전용 프로퍼티**

```tsx
interface Person {
  readonly name: string;
}
```

**메서드 타입 정의**

```tsx
// 함수 타입 표현식
interface Person {
  readonly name: string;
  age?: number;
  sayHi: () => void;;
}

// 호출 시그니쳐
interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void;
}
```

**메서드 오버로딩**

함수 타입 표현식으로 메서드의 타입을 정의하면 메서드 오버로딩 구현 불가능. ⇒ 호출 시그니처 사용해야 함.

```tsx
interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void;
  sayHi(a: number): void;
  sayHi(a: number, b: number): void;
}
```

**하이브리드 타입**

```tsx
interface Func2 {
  (a: number): string;
  b: boolean;
}

const func: Func2 = (a) => "hello";
func.b = true;
```

📌 **주의할 점**

인터페이스에서는 Union이나 Intersection 타입을 정의할 수 없음.

타입 별칭과 함께 사용하거나, 타입 주석에서 직접 사용해야 함.

```tsx
type Type1 = number | string | Person;
type Type2 = number & string & Person;

const person: Person & string = {
  name: "이정환",
  age: 27,
};
```

## 인터페이스 확장하기

### 인터페이스 확장

하나의 인터페이스를 다른 인터페이스들이 상속받아 중복된 프로퍼티를 정의하지 않도록 도와주는 문법.

```tsx
interface Animal {
  name: string;
  color: string;
}

interface Dog extends Animal { // Animal은 Dog타입의 슈퍼타입.
  breed: string;
}

interface Cat extends Animal {
  isScratch: boolean;
}

interface Chicken extends Animal {
  isFly: boolean;
}
```

**프로퍼티 재 정의하기**

```tsx
interface Animal {
  name: string;
  color: string;
}

interface Dog extends Animal {
  name: "doldol"; // 타입 재 정의
  breed: string;
}
```

🚨 프로퍼티를 재 정의 할때는 반드시 원본 타입의 서브 타입이 되도록 정의해야 함.

ex) name을 Number 타입으로 재 정의 불가능.

**타입 별칭 확장**

인터페이스는 타입 별칭으로 정의된 객체도 확장 가능.

```tsx
type Animal = {
  name: string;
  color: string;
};

interface Dog extends Animal {
  breed: string;
}
```

**다중 확장**

여러개의 인터페이스 확장 가능.

```tsx
interface DogCat extends Dog, Cat {}

const dogCat: DogCat = {
  name: "",
  color: "",
  breed: "",
  isScratch: true,
};
```

## 인터페이스 선언 합치기

### 선언 합침

중복된 이름의 인터페이스 선언은 하나로 합쳐짐. 

➕ 타입 별칭의 경우 동일 스코프 내에서 중복된 이름으로 선언할 수 없음.

```tsx
interface Person {
  name: string;
}

interface Person { // ✅
  age: number;
}

// 합쳐져서 아래와 같은 인터페이스가 됨.
interface Person {
	name: string;
	age: number;
}
```

🚨 동일한 이름의 인터페이스들이 동일한 이름의 프로퍼티를 서로 다른 타입으로 정의할 경우 오류 발생. ⇒ **충돌**!

# 7. 클래스

동일한 모양의 객체를 더 쉽게 생성하도록 도와주는 문법. 객체를 생성하는 틀.

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

  // 메서드
  study() {
    console.log("열심히 공부 함");
  }

  introduce() {
    console.log(`안녕하세요!`);
  }
}

let studentB = new Student("홍길동", "A+", 27);

studentB.study(); // 열심히 공부 함
studentB.introduce(); // 안녕하세요!
```

**this**

클래스 내부에서 현재 만들고 있는 객체를 의미.

```tsx
class Student {
  (...)

  introduce() {
    console.log(`안녕하세요 ${this.name} 입니다!`);
  }
}

let studentB = new Student("홍길동", "A+", 27);

studentB.introduce(); // 안녕하세요 이정환 입니다!
```

**상속**

상속 받을때는 super 메서드를 사용해서 확장 대상 클래스의 생성자를 호출해야 함.

```tsx
// StudentDeveloper 클래스는 Student 클래스에 정의된 모든 필드, 메서드를 가짐.
class StudentDeveloper extends Student {
  // 필드
  favoriteSkill;

  // 생성자
  constructor(name, grade, age, favoriteSkill) {
	  // super 호출
    super(name, grade, age);
    this.favoriteSkill = favoriteSkill;
  }

  // 메서드
  programming() {
    console.log(`${this.favoriteSkill}로 프로그래밍 함`);
  }
}
```

## 타입 스크립트의 클래스

타입스크립트에서는 클래스의 필드를 선언할 때 타입 주석으로 타입을 함께 정의해야 함.

각 필드의 값을 초기화 하지 않을 경우 초기값도 함께 명시해야 함.

```tsx
class Employee {
  // 필드
  name: string = "";
  age: number = 0;
  position?: string = ""; // 선택적 프로퍼티

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

**클래스는 타입**

클래스를 타입으로 사용하면 해당 클래스가 생성하는 객체의 타입과 동일한 타입이 됨.

```tsx
class Employee {
  (...)
}

const employeeC: Employee = { // 타입을 Employee 클래스로 정의.
  name: "",
  age: 0,
  position: "",
  work() {},
};
```

**상속**

타입스크립트에서 클래스의 상속을 이용할 때 파생 클래스에서 생성자를 정의했다면 super 메서드로 슈퍼클래스의 생성자를 호출해야 하며, 호출 위치는 생성자의 최상단이어야 함.

```tsx
class ExecutiveOfficer extends Employee {
  officeNumber: number;

  constructor(
    name: string,
    age: number,
    position: string,
    officeNumber: number
  ) {
    super(name, age, position);
    this.officeNumber = officeNumber;
  }
}
```

## 접근 제어자

타입스크립트에서만 제공되는 기능으로 클래스의 특정 필드나 메서드를 접근할 수 있는 범위를 설정.

- **public** : 모든 범위에서 접근 가능
- **private** : 클래스 내부에서만 접근 가능
- **proteced** : 클래스 내부 또는 파생 클래스 내부에서만 접근 가능

### public

필드의 접근 제어자를 지정하지 않으면 기본적으로 public.

```tsx
class Employee {
  // 필드
  name: string;
  age: number;
  public position: string; // 직접 명시도 가능

  ...
}
```

### private

특정 필드나 제어자를 private로 설정하면 클래스 내부에서만 필드에 접근 가능. 외부에서는 접근 불가능.

```tsx
class Employee {
  // 필드
  private name: string; // private 접근 제어자 설정
  public age: number;
  public position: string;

  ...

  // 메서드
  work() {
    console.log(`${this.name}이 일함`); // 여기서는 접근 가능
  }
}

const employee = new Employee("이정환", 27, "devloper");

employee.name = "홍길동"; // ❌ 오류
employee.age = 30;
employee.position = "디자이너";
```

### protected

private와 public의 중간으로, 클래스 외부에서는 접근 안되지만 클래스 내부와 **파생클래스**에서는 접근 가능.

```tsx
class Employee {
  // 필드
  private name: string; // private 접근 제어자 설정
  protected age: number;
  public position: string;

  ...

  // 메서드
  work() {
    console.log(`${this.name}이 일함`); // 여기서는 접근 가능
  }
}

// Employee를 상속받는 파생 클래스
class ExecutiveOfficer extends Employee {
 // 메서드
  func() {
    this.name; // ❌ 오류 
    this.age; // ✅ 가능
  }
}

const employee = new Employee("이정환", 27, "devloper");

employee.name = "홍길동"; // ❌ 오류
employee.age = 30; // ❌ 오류
employee.position = "디자이너";
```

### 필드 생략하기

생성자의 매개변수에 접근 제어자 설정 가능.

자동으로 필드가 함께 선언되므로, 동일한 이름으로 필드 선언을 하지 못하게 됨.

접근 제어자가 설정된 매개변수들은 `this.필드 = 매개변수가` 자동으로 수행.

⇒ **생성자 매개변수에 접근 제어자를 설정하여 필드 선언, 생성자 내부 코드를 생략 가능!**

```tsx
class Employee {
  // 생성자
  constructor(
    private name: string,
    protected age: number,
    public position: string
  ) {}

  // 메서드
  work() {
    console.log(`${this.name} 일함`);
  }
}
```

## 인터페이스와 클래스

인터페이스는 클래스에 어떤 필드와 메서드가 존재하는지 정의 가능. → 클래스의 설계도 역할을 할 수 있음.

```tsx
interface CharacterInterface {
  name: string;
  moveSpeed: number;
  move(): void;
}

// 이 클래스가 생성하는 객체는 모두 인터페이스 타입을 만족해야 함.
class Character implements CharacterInterface {
  constructor(
    public name: string,
    public moveSpeed: number,
    private extra: string
  ) {}

  move(): void {
    console.log(`${this.moveSpeed} 속도로 이동!`);
  }
}
```

# 8. 제네릭

함수, 인터페이스, 타입 별칭, 클래스 등을 다양한 타입과 함께 동작하도록 만들어주는 기능.

### 제네릭이 필요한 상황

```tsx
function func(value: any) {
  return value;
}

let num = func(10);
// any 타입

let str = func("string");
// any 타입

num.toUpperCase() // 오류가 발생하지 않음.
```

인수로 전달한 타입 그대로 반환 값의 타입이 설정되었으면 좋겠음. → 제네릭 함수로 만들기.

### 제네릭 함수

두루두루 모든 타입의 값을 다 적용할 수 있는 범용적인 함수.

```tsx
function func<T>(value: T): T {
  return value;
}

let num = func(10);
// number 타입
```

타입 변수 T에 어떤 값이 할당될 지는 **함수가 호출될 때 결정**.

타입 변수에 할당할 타입을 직접 명시하는 것도 가능.

```tsx
function func<T>(value: T): T {
  return value;
}

let arr = func<[number, number, number]>([1, 2, 3]);
```

**두 개의 타입 변수가 필요할 때**

```tsx
function swap<T, U>(a: T, b: U) {
  return [b, a];
}

const [a, b] = swap("1", 2);
```

**다양한 배열 타입을 인수로 받는 제네릭 함수를 만들어야 할 때**

```tsx
function returnFirstValue<T>(data: T[]) { // 배열이 아닌 값은 전달 불가능.
  return data[0];
}

let num = returnFirstValue([0, 1, 2]);
// number

let str = returnFirstValue([1, "hello", "mynameis"]);
// number | string
```

**반환 값의 타입을 배열의 첫번째 요소의 타입이 되도록 할 때**

```tsx
function returnFirstValue<T>(data: [T, ...unknown[]]) {
  return data[0];
}

let str = returnFirstValue([1, "hello", "mynameis"]);
// number
```

**타입 변수를 제한할 때**

```tsx
function getLength<T extends { length: number }>(data: T) {
  return data.length;
}

getLength("123");            // ✅ string에는 length 타입의 프로퍼티가 있음.

getLength([1, 2, 3]);        // ✅ 배열에는 length 타입의 프로퍼티가 있음.

getLength({ length: 1 });    // ✅ length 프로퍼티가 존재하는 객체를 전달함.

getLength(undefined);        // ❌

getLength(null);             // ❌
```

T는 `{ length : number }` 객체 타입의 서브 타입. 무조건 Number 타입의 프로퍼티 length를 가져야 함.

## Map, ForEach 메서드 타입 정의

### Map

원본 배열의 타입, 새롭게 반환하는 배열의 타입 다르게 정의해야 함.

```tsx
const arr = [1, 2, 3];

function map<T, U>(arr: T[], callback: (item: T) => U): U[] {
  (...)
}

map(arr, (it) => it.toString());
// string[] 타입의 배열을 반환
// 결과 : ["1", "2", "3"]
```

### ForEach

반환 값이 없는 메서드이므로 반환값 타입 void로 정의.

```tsx
function forEach<T>(arr: T[], callback: (item: T) => void) {
  for (let i = 0; i < arr.length; i++) {
    callback(arr[i]);
  }
}
```

## 제네릭 인터페이스, 제네릭 타입 별칭

### 제네릭 인터페이스

제네릭 인터페이스는 변수의 타입으로 정의할 때 `<>` 에 타입 변수에 할당할 타입을 명시해야 함.

```tsx
interface KeyPair<K, V> {
  key: K;
  value: V;
}

let keyPair: KeyPair<string, number> = {
  key: "key",
  value: 0,
};

let keyPair2: KeyPair<boolean, string[]> = {
  key: true,
  value: ["1"],
};
```

인덱스 시그니쳐와 함께 사용

```tsx
interface Map<V> {
  [key: string]: V;
}

// V가 string이므로 key가 string, value가 string인 모든 프로퍼티를 포함하는 객체 타입이 됨.
let stringMap: Map<string> = {
  key: "value",
};

let booleanMap: Map<boolean> = {
  key: true,
};
```

### 제네릭 타입 별칭

타입 별칭에 제네릭을 적용할 때도 변수의 타입으로 정의할 때 `<>` 에 타입 변수에 할당할 타입을 명시해야 함.

```tsx
type Map2<V> = {
  [key: string]: V;
};

let stringMap2: Map2<string> = {
  key: "string",
};
```

```tsx
interface Student {
  type: "student";
  school: string;
}

interface Developer {
  type: "developer";
  skill: string;
}

// 제네릭 인터페이스
interface User<T> {
  name: string;
  profile: T;
}

// 타입 좁히기를 할 필요 없이 학생만 할 수 있는 기능을 정의할 수 있음.
function goToSchool(user: User<Student>) {
  const school = user.profile.school;
  console.log(`${school}로 등교 완료`);
}

/* 제네릭 인터페이스를 사용하지 않을 경
function goToSchool(user: User<Student>) {
  if (user.profile.type !== "student") {
    console.log("잘 못 오셨습니다");
    return;
  }

  const school = user.profile.school;
  console.log(`${school}로 등교 완료`);
}
*/

const developerUser: User<Developer> = {
  name: "이정환",
  profile: {
    type: "developer",
    skill: "TypeScript",
  },
};

const studentUser: User<Student> = {
  name: "홍길동",
  profile: {
    type: "student",
    school: "가톨릭대학교",
  },
};
```

## 제네릭 클래스

제네릭 클래스를 사용해 여러 타입을 다루는 범용적 클래스를 정의할 수 있음.

```tsx
class List<T> {
  constructor(private list: T[]) {}

  push(data: T) {
    this.list.push(data);
  }

  pop() {
    return this.list.pop();
  }

  print() {
    console.log(this.list);
  }
}

// 타입이 자동으로 추론됨
const numberList = new List([1, 2, 3]);
const stringList = new List(["1", "2"]);

// 직접 타입을 설정하는 경우
const numberList = new List<number>([1, 2, 3]);
const stringList = new List<string>(["1", "2"]);
```

## 프로미스와 제네릭

프로미스는 제네릭 클래스로 구현되어 있음. → Promise 생성 시 타입 변수에 할당할 타입을 직접 설정할 수 있음. 해당 타입이 resolve의 결과 타입.

```tsx
const promise = new Promise((resolve, reject) => resolve(45))

promise.then(result => result * 4)  // Error. result의 type이 {}로 추론됨.

const promise = new Promise<number>((resolve, reject) => resolve(45))

promise.then(result => result * 4)  // 180
```

```tsx
// result의 타입을 number로 설정
const promise = new Promise<number>((resolve, reject) => {
  setTimeout(() => {
    // 결과값 : 20
    resolve(20);
  }, 3000);
});

promise.then((response) => {
  // response는 number 타입
  console.log(response);
});

// reject 함수에 인수로 전달하는 값은 타입 정의 불가능. 타입 좁히기 사용해야 함.
promise.catch((error) => {
  if (typeof error === "string") {
    console.log(error);
  }
});
```

# 9. 타입 조작하기

원래 존재하던 타입들을 상황에 따라 유동적으로 다른 타입으로 변환하는 기능.

- 제네릭
- 조건부 타입
- 인덱스드 엑세스 타입
- keyof 연산자
- Mapped 타입
- 템플릿 리터럴 타입

## 인덱스드 엑세스 타입

인덱스를 이용해 다른 타입 내의 특정 프로퍼티의 타입을 추출하는 타입.

객체, 배열, 튜플에 사용 가능.

### 객체

`Post["author"]` Post 타입으로부터 author(인덱스) 프로퍼티의 타입을 추출.

author 매개변수의 타입은 `{id : number, name: string, age:number}` 가 됨.

🚨 인덱스에는 값이 아니라 타입만 들어갈 수 있음. 인덱스에 존재하지 않는 프로퍼티 이름을 쓰면 오류 발생.

```tsx
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number; // 추가
  };
}
 
function printAuthorInfo(author: Post["author"]) {
  console.log(`${author.id} - ${author.name}`);
}

// 인덱스 중첩 사용 가능.
function printAuthorInfo(author: Post["author"]['id']) {
	// author 매개변수의 타입은 number 타입이 됨
  console.log(`${author.id} - ${author.name}`);
}
```

### 배열

PostList 배열 타입에서 하나의 요소의 타입만 뽑아올 수 있음.

Number 타입 또는 Number Literal 타입을 넣을 수 있음.

```tsx
type PostList = {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number;
  };
}[];

const post: PostList[number] = {
  title: "게시글 제목",
  content: "게시글 본문",
  author: {
    id: 1,
    name: "이정환",
    age: 27,
  },
};
```

### 튜플

```tsx
type Tup = [number, string, boolean];

type Tup0 = Tup[0];
// number

type Tup1 = Tup[1];
// string

type Tup2 = Tup[2];
// boolean

type Tup3 = Tup[number]
// 튜플을 배열처럼 인식해서 배열 요소의 타입을 추출함.
// number | string | boolean
```

## Keyof 연산자

객체 타입으로부터 프로퍼티의 모든 key들을 String Literal Union 타입으로 추출하는 연산자.

타입에만 사용할 수 있는 연산자.

`keyof 타입` 형태로 사용.

 `타입`의 모든 프로퍼티 key를 String Literal Union 타입으로 추출.

 `keyof Person`의 결과값은 `“name” | “age” | “location”` .

```tsx
interface Person {
  name: string;
  age: number;
  location: string; // 추가
}

function getPropertyKey(person: Person, key: keyof Person) {
  return person[key];
}

const person: Person = {
  name: "이정환",
  age: 27,
};
```

typeof 연산자는 특정 변수의 타입을 추론하는 기능을 가지고 있으므로, 아래와 같이 keyof와 함께 사용 가능.

```tsx
(...)

function getPropertyKey(person: Person, key: keyof typeof person) {
  return person[key];
}

const person: Person = {
  name: "이정환",
  age: 27,
};
```

## Mapped 타입

기존 객체 타입을 기반으로 새로운 객체 타입을 만듦.

K에 있는 값을 in 오퍼레이터로 순회한 값을 P로 두고, P는 T로 변환함.

```tsx
{ [P in K] : T }
{ [P in K] ? : T }
{ readonly [ P in K ] : T }
{ readonly [ P in K ] : T }
```

```tsx
interface User {
  id: number;
  name: string;
  age: number;
}

type PartialUser = {
  [key in "id" | "name" | "age"]?: User[key];
};

(...)
```

- key가 “id” 일 때 → `id : User[id]` → `id : number`
- key가 “name”일 때 → `name : User[user]` → `name : string`
- key가 “age”일 때 → `age : User[age]` → `age : number`

결과적으로 아래와 같은 타입

```tsx
{
  id?: number;
  name?: string;
  age?: number;
}
```

keyof, readonly 활용

```tsx
interface User {
  id: number;
  name: string;
  age: number;
}

type PartialUser = {
  [key in keyof User]?: User[key];
};

type ReadonlyUser = {
  readonly [key in keyof User]: User[key];
};

(...)
```

## 템플릿 리터럴 타입

템플릿 리터럴을 이용해 특정 패턴을 갖는 String 타입을 만드는 기능.

```tsx
type Color = "red" | "black" | "green";
type Animal = "dog" | "cat" | "chicken";

// Color와 Animal을 조합해 만들 수 있는 모든 가지 수
// type ColoredAnimal = `red-dog` | 'red-cat' | 'red-chicken' | 'black-dog' ... ;
type ColoredAnimal = `${Color}-${Animal}`;
```