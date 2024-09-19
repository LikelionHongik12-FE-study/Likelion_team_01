# Section 1~4

---

## Typescript란 무엇인가?

---

타입스크립트란 자바스크립트에서 타입 관련 기능들을 추가한 프로그래밍 언어

→ 이는 자바스크립트의 유연함으로 인해 대규모 프로젝트를 컨트롤하기 힘든 단점을 보완

“타입” 기능을 추가함으로써 보다 안정적으로 자바스크립트를 사용할 수 있음

## 타입 시스템 (Type System)

---

크게 동적 타입 시스템과 정적 타입 시스템이 있음

**정적 타입 시스템**

코드를 실행하기 전에 정적으로 변수의 타입을 결정 → 오류 발생 시 프로그램 실행 불가

**(단점) 모든 변수에 일일이 타입을 지정해줘야하는 번거로움**

**동적 타입 시스템**

코드를 실행하면서 유동적으로 변수의 타입을 결정 → 런타임 이후에 오류 발생 여부 판단

**(단점) 코드의 타입 오류를 미리 검사할 수 없음 / 예기치 못한 오류 발생 가능**

→ 타입스크립트는 이 두가지 타입의 장점을 혼합한 점진적 타입 시스템을 사용

(변수의 초기화 값으로 타입 추론 + 직접 타입 지정 가능)

## Compile Process

---

**대다수의 프로그래밍 언어의 동작 과정**

ex.) JavaScript → AST(추상 문법 트리) → 바이트 코드

![스크린샷 2024-09-10 오후 3.54.22.png](Section%201~4%207dcaca1dc1cf4f25aafbdf74563b84cd/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-09-10_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.54.22.png)

**타입스크립트의 컴파일 과정**

![스크린샷 2024-09-10 오후 3.57.20.png](Section%201~4%207dcaca1dc1cf4f25aafbdf74563b84cd/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-09-10_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.57.20.png)

![스크린샷 2024-09-10 오후 3.57.09.png](Section%201~4%207dcaca1dc1cf4f25aafbdf74563b84cd/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-09-10_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.57.09.png)

→ 타입 검사를 통과하지 못하면 자바스크립트 파일이 생성되지 않으므로

타입스크립트에서 컴파일된 자바스크립트 코드는 비교적 안전한 코드

## 기본 세팅

---

1. **node.js 패키지 초기화**
    
    ```bash
    npm init
    ```
    
2. **@types/node 패키지 설치**
    
    ```bash
    npm install @types/node
    ```
    

1. **Typescript 컴파일러(TSC) 설치**
    
    ```bash
    // 전역 설치
    npm install typescript -g
    
    // 프로젝트 내 설치
    npm install --save-dev typescript
    npm install -D typescript
    ```
    
    → save dev 옵션으로 설치하는 것이 더 권장되는 방법
    

1. **tsx 라이브러리 설치**
    
    ```bash
    npm install tsx --save-dev
    ```
    

## TSC(타입스크립트 컴파일러) 옵션 설정하기

---

1. **`include`**
    
    TypeScript 컴파일러가 **어떤 파일**을 컴파일해야 할지 지정하는 역할
    
    기본적으로 **`tsconfig.json`** 파일에서 설정되며, 배열 형태로 파일 경로나 패턴을 명시할 수 있음
    
    ```json
    {
      "include": ["src"]
    }
    ```
    
2. **`compilerOptions`**
    1. **target - 컴파일할 자바스크립트의 타겟 버전 설정**
    2. **module - 컴파일할 자바스크립트의 모듈 시스템 설정**
    3. **outDir - 컴파일된 JavaScript 파일이 담길 폴더 설정**
    4. **strict - 타입 추론 불가능할 때 오류 발생**
    5. **moduleDetection - 타입스크립트가 각각의 파일을 어떤 모듈로 감지할 것인지 설정**
        
        기본적으로 모든 타입스크립트 파일은 글로벌 모듈로 취급
        
        → 같은 레벨의 스코프에 같은 이름 변수 생성 시 오류 발생
        
        **해결 방법**
        
        ```tsx
        export {}
        import {}
        ```
        
        **→ 이를 포함하는 것도 방법인데, 이를 타입스크립트를 컴파일하면 자바스크립트 파일 안에 생성됨**
        
    6. **ts-node > esm**
        
        **tsx는 기본적으로 CommonJS 형식을 이해하므로, ESM 형식을 이해하도록 설정 필요**
        
        ```json
        "ts-node": { "esm": true }
        ```
        

---

## 원시 타입 (Primitive Type)

---

하나의 값만 저장하는 타입

ex.) number, string, boolean, undefined, null

**Example)**

```tsx
let num1: number = 123;
```

**`: number` 와 같이 변수의 이름 뒤에 콜론과 함께 타입을 작성하는 것을 type annotation(주석)이라 함**

→ 이 방식이 타입스크립트에서 변수의 타입을 정의하는 가장 기본적인 방식

1. **number**
    
    ```tsx
    let num1: number = 123;
    let num2: number = -123;
    let num3: number = 0.123;
    let num4: number = -0.123;
    let num5: number = Infinity;
    let num6: number = -Infinity;
    let num7: number = NaN;
    ```
    
2. **string**
    
    ```tsx
    let str1: string = 'hello';
    let str2: string = "hello";
    let str3: string = `hello`;
    let str4: string = `hello ${num1}`
    ```
    
3. **boolean**
    
    ```tsx
    let bool1: boolean = true;
    let bool2: boolean = false;
    ```
    
4. **undefined**
    
    ```tsx
    let unde1: undefined = undefined;
    ```
    
5. **null**
    
    ```tsx
    let null1: null = null;
    ```
    

## 리터럴 타입 (Literal Type)

---

**변수의 타입을 값 그 자체로 정의함**

```tsx
let numA: 10 = 10;
```

→ 타입으로 정의된 값 외에는 다른 값을 할당할 수 없음

문자열의 경우에도 리터럴 타입이 존재

```tsx
let strA: "hello" = "hello";
```

Boolean 타입의 경우

```tsx
let boolA: true = false; // 오류 발생
```

**→ 이러한 Literal Type은 복합적인 타입들을 만들 때 유용하게 사용됨**

## 배열과 튜플

---

### 배열(Array)

---

1. 일반적인 방식의 배열 타입 정의
    
    ```tsx
    let numArr: number[] = [1, 2, 3];
    
    let strArr: string[] = ["a", "b", "c"];
    ```
    
2. 제네릭 문법 사용
    
    ```tsx
    let boolArr: Array<boolean> = [true, false, true];
    ```
    

1. 요소들의 타입이 다양할 경우
    
    ```tsx
    let multiArr: (string | number)[] = [1, "hello"];
    ```
    
2. 다차원 배열
    
    ```tsx
    let doubleArr = [
      [1, 2, 3],
      [4, 5],
    ];
    ```
    

### 튜플(Tuple)

---

**길이와 타입이 고정된 배열**

```tsx
let tup1: [number, number] = [1, 2];

tup1 = ["1", "2"]; // 타입 불일치 (오류 발생)

tup1 = [1, 2, 3]; // 길이 불일치 (오류 발생)
```

**배열 메소드 사용**

```tsx
tup1.push(2);
tup1.pop();
tup1.pop();
tup1.pop();
```

**타입스크립트에서 튜플은 자바스크립트로 컴파일 됐을 때,**

**배열로 취급되므로 배열 메소드를 적용하는 것에 대해서는 제한이 없음**

**활용 예시**

```tsx
const users: [string, number][] = [
  ["김철흥", 1],
  ["임승민", 2],
  ["최재백", 3],
  ["김지원", 4],
  ["나상혁", 5],
];
```

**→ `users` 변수는 `[string, number]` 튜플들을 요소로 가지는 배열**

**→ 다차원 배열에 접근할 때, `[string,number][]` 로 작성하는 이유는 `number[]` 의 형식과 유사함**

## 객체(Object)

---

### object 타입 사용

---

```tsx
let user: object = {
  id: 1,
  name: "김철흥",
};

user.id; // Property 'id' does not exist on type 'object'
```

→ **`object` 타입은 해당 변수가 객체인 것 외에 키와 프로퍼티 값에 대한 정보가 없음**

→ **`user.id` 와 같이 점 표기법으로 객체에 접근하려고 하면 키 값이 존재하지 않는다는 오류 발생**

**따라서, 객체를 온전하게 사용하기 위해서는 객체 리터럴 타입으로 변수를 정의하는게 좋음!**

### 객체 리터럴 타입 사용

---

객체의 모든 프로퍼티들의 타입까지 구조적으로 정의할 수 있음

```tsx
let user: {
  id: number;
  name: string;
} = {
  id: 1,
  name: "김철흥",
};
```

**타입스크립트에서는 객체의 구조를 기준으로 객체를 정의함**

**→  구조적 타입 시스템 (Property-based type system) 사용**

✶ C, Java와 같은 언어는 명목적 타입 시스템(nominative type system) 사용

→ 자료형이 자료형의 이름에 의해 명시적으로 결정됨

### 옵셔널 프로퍼티 (Optional Property)

---

**프로퍼티의 이름 뒤에 `?` 를 추가**

→ 프로퍼티의 유무는 상관 없이 동작하지만, 값이 존재한다면 특정 타입이여야 함을 의미

```tsx
let user: {
  id?: number;
  name: string;
} = {
  name: "김철흥",
};
```

### `readonly` 프로퍼티

---

**프로퍼티의 값을 변경하지 못하게끔 설정**

```tsx
let config: {
  readonly apiKey: string;
} = {
  apiKey: "dasjkfb",
};

config.apiKey = "hacked"; // Cannot assign to 'apiKey' because it is a read-only property.
```

## 타입 별칭 (Type alias)

---

**타입 정의를 마치 변수처럼 하도록 도와주는 문법 (for 재사용)**

→ 같은 스코프 내 동일한 이름의 타입 별칭을 선언하면 오류 발생

```tsx
type User = {
  id: number;
  name: string;
  nickname: string;
  birth: string;
  bio: string;
  location: string;
};

type User = {
	....
} // 동일한 이름의 타입 별칭 사용으로 인한 오류 발생

let user: User = {
  id: 1,
  name: "김철흥",
  nickname: "DefineX",
  birth: "2001.01.16",
  bio: "안녕하세요",
  location: "서울시",
};
```

## 인덱스 시그니처

---

키와 값의 규칙을 기준으로 객체의 타입을 정의하는 문법

```tsx
type CountryCodes = {
    [key: string] : string;
}

let countryCodes: CountryCodes = {
    Korea: 'ko',
    UnitedStates: 'us',
    UnitedKingdom: 'uk',
}
```

인덱스 시그니처는 key와 value 값에 대한 규칙을 위반할 때 오류 발생

→ 빈 객체일 경우에는, 이를 위반할 값들이 없으므로 오류 발생하지 않음

```tsx
type CountryNumberCodes = {
  [key: string]: number;
  Korea: number;
};

let countryNumberCodes: CountryNumberCodes = {
  Korea: 410,
};
```

→ **`Korea: number` 와 같은 추가적인 프로퍼티를 정의해놓으려면,**

**추가적인 프로퍼티의 value 타입과 인덱스 시그니처의 value 타입이 일치 or 호환되어야함**

```tsx
type CountryNumberCodes = {
  [key: string]: number;
  Korea: string; // value 타입 불일치로 인한 오류 발생
};
```

## Enum 타입

---

여러가지 값들에 각각 이름을 부여해 열거해두고 사용하는 타입

→ 데이터의 정의를 효율적으로 관리할 수 있음 (가독성 측면에서도)

**숫자형 Enum**

이러한 Enum은 숫자를 자동으로 할당시킬 수 있고, 시작하는 숫자를 직접 지정할 수도 있음

```tsx
enum Role {
  ADMIN,
  USER,
  GUEST,
}

const user1 = {
  name: "김철흥",
  role: Role.ADMIN,
};

const user2 = {
  name: "이준완",
  role: Role.USER,
};

const user3 = {
  name: "윤영준",
  role: Role.GUEST,
};
```

→ 값을 지정해주지 않으면 0을 시작으로 각 요소에 숫자 자동 할당됨 (0, 1, 2, …)

**출력 결과**

```bash
{ name: '김철흥', role: 0 } { name: '이준완', role: 1 } { name: '윤영준', role: 2 }
```

**문자형 Enum**

```tsx
enum Language {
  korean = "ko",
  english = "en",
}
```

<aside>
📌

**참고 사항**

Enum 타입은 자바스크립트로 컴파일 됐을 때, 자바스크립트의 객체로 변환됨 **(사라지지 않음!)**

**따라서, 코드에서 값을 사용하듯이 사용할 수 있음**

**Typescript Enum 코드**

```tsx
enum Role {
  ADMIN,
  USER,
  GUEST,
}

enum Language {
  korean = "ko",
  english = "en",
}
```

**컴파일된 후 JavaScript 코드**

```jsx
var Role;
(function (Role) {
    Role[Role["ADMIN"] = 0] = "ADMIN";
    Role[Role["USER"] = 1] = "USER";
    Role[Role["GUEST"] = 2] = "GUEST";
})(Role || (Role = {}));

var Language;
(function (Language) {
    Language["korean"] = "ko";
    Language["english"] = "en";
})(Language || (Language = {}));
```

</aside>

## Any 와 Unknown 타입

---

### any

---

특정 변수의 타입을 확실히 알지 못할 때 사용

```jsx
let anyVar: any = 10;

let num: number = 10;
num = anyVar;

anyVar = true;
anyVar = {};
anyVar = () => {};

anyVar.toUpperCase(); // anyVar이 함수형으로 바뀌었으므로 런타임에 오류 발생
anyVar.toFixed(); // Same as previous case
```

→ 어떤 타입의 값도 담을 수 있고, 반대로 타입 정의가 된 변수의 값으로 할당도 가능

→ 특정 자료형 메소드 사용에도 제약 없음

### Unknown

---

어떤 타입의 값도 변수에 담을 수 있지만,

**`any` 와는 다르게 타입 정의가 된 변수에는 값을 할당할 수 없음**

```tsx
let unknownVar: unknown;

let num: number = 10;

num = unknownVar; // Type 'unknown' is not assignable to type 'number'.

unknownVar = "";
unknownVar = 1;
unknownVar = () => {};

// 타입 정제 (타입 좁히기)
if(typeof unknownVar === 'number') {
    num = unknownVar;
}
```

→ 타입 정제 기능을 구현할 때 사용

## Void 와 Never

---

### Void

---

아무런 값을 반환하지 않는 경우에 사용하는 타입

**Example)**

```tsx
function func2(): void {
  console.log("hello");
}
```

**변수에 void 타입을 사용하는 경우에는, undefined를 제외한 어떤 값도 할당할 수 없음**

```tsx
let a: void;
a = 1; // Type 'number' is not assignable to type 'void'
a = "hello"; // Type 'string' is not assignable to type 'void'
a = {}; // Type '{}' is not assignable to type 'void'
a = null; // Type 'null' is not assignable to type 'void'
a = undefined; // 오류 발생 X
```

<aside>
📌

**Q. 왜 기존에 존재하는 undefined 나 null 타입을 사용하지 않을까?!**

**A. `void` 는 반환값이 아예 없다는 의미로, 반환을 신경쓰지 않는다는 더 추상적인 개념**

반면, **`undefined`** 와 **`null`**은 실제 값이므로, 이를 반환해야하는 경우에만 사용

**`void`**는 구체적인 값과는 차별화된 개념 (**`void`**로 표현하는 것이 의미가 더 명확함)

1. **undefined**를 함수의 반환값으로 사용하는 경우
    
    ```tsx
    function func2(): undefined {
      console.log("hello");
    }
    ```
    
    → **`undefined`**의 경우에는 오류 발생하지 않지만,
    
    **`void`**를 사용하는 것이 의미론적으로 직관성과 명확성이 높아지므로
    
    함수의 반환값이 없을 때는 **`void`** 타입으로 함수 타입을 정의하는게 바람직함
    
2. **null**을 함수의 반환 값으로 사용하는 경우
    
    ```tsx
    function func2(): null {
      console.log("hello");
      return; // Type 'undefined' is not assignable to type 'null'
    }
    ```
    
    → **`return;` 는 `return: undefined;` 와 동일한 코드
    
    오류를 해결하기 위해서는 다음과 같이 return 값으로 `null`을 명시해줘야함!**
    
    ```tsx
    function func2(): null {
      console.log("hello");
      return null;
    }
    ```
    
</aside>

### Never

---

존재하지 않는, 불가능한 타입

→ 이 또한, 의미론적인 이유에서 무한루프나 **`throw Error`** 와 같은 함수에 사용

```tsx
function func3(): never {
  while (true) {}
}

function func4(): never {
    throw new Error();
}
```

**변수에 never 타입을 정의하면, 그 어떠한 타입의 값도 할당할 수 없음**

```tsx
let anyVar: any;

let a: never;
a = 1; // Type 'number' is not assignable to type 'never'
a = "hello"; // Type 'string' is not assignable to type 'never'
a = {}; // Type '{}' is not assignable to type 'never'
a = null; // Type 'null' is not assignable to type 'never'
a = undefined; // Type 'undefined' is not assignable to type 'never'
a = anyVar; // Type 'any' is not assignable to type 'never'
```

## 타입의 집합 관계

---

### 타입 호환성

---

**어떤 타입을 다른 타입으로 취급해도 괜찮은지 판단하는 것**

![스크린샷 2024-09-11 오후 5.12.12.png](Section%201~4%207dcaca1dc1cf4f25aafbdf74563b84cd/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-09-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.12.12.png)

**→ 슈퍼타입의 값을 서브타입의 값으로 취급할 수는 없지만, 서브타임의 값을 슈퍼타입의 값으로 취급할 수 있음**

**이를 각각 Upcast 와 Downcast 라고 함**

![스크린샷 2024-09-11 오후 5.14.50.png](Section%201~4%207dcaca1dc1cf4f25aafbdf74563b84cd/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-09-11_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_5.14.50.png)

## 타입 계층도

---

**타입 간의 부모-자식 관계를 도식화한 계층도**

![타입계층도.png](Section%201~4%207dcaca1dc1cf4f25aafbdf74563b84cd/%25E1%2584%2590%25E1%2585%25A1%25E1%2584%258B%25E1%2585%25B5%25E1%2586%25B8%25E1%2584%2580%25E1%2585%25A8%25E1%2584%258E%25E1%2585%25B3%25E1%2586%25BC%25E1%2584%2583%25E1%2585%25A9.png)

→ any 타입은 예외적으로 이러한 관계와 무관하게 동작

**(But, never 타입 변수에는 값을 할당할 수 없음 (never 타입에 Downcast 불가능))**

## 객체 타입의 호환성

---

공통된 키 값 이외의 추가적인 프로퍼티가 있는 객체가 더 구체적인 객체

**더 넓은 범위의 객체  - 슈퍼타입 / 더 좁은 범위의 객체  - 서브타입**

```tsx
type Animal = {
  name: string;
  color: string;
};

type Dog = {
  name: string;
  color: string;
  breed: string;
};

let animal: Animal = {
  name: "기린",
  color: "yellow",
};

let dog: Dog = {
    name: '돌돌이',
    color: "brown",
    breed: "진도"
}

animal = dog;

dog = animal; // Property 'breed' is missing in type 'Animal' but required in type 'Dog'
```

<aside>
📌

**초과 프로퍼티 검사**

**객체 리터럴 타입으로 객체를 정의하는 경우에는 존재하지 않는 프로퍼티에 대한 타입을 검사**

**→ 객체 타입의 변수를 초기화할 때 검사가 실행됨**

```tsx
type Book = {
  name: string;
  price: number;
};

type ProgrammingBook = {
  name: string;
  price: number;
  skill: string;
};

let book2: Book = {
  name: "한입 크기로 잘라먹는 리액트",
  price: 33000,
  skill: "reactjs", // Object literal may only specify known properties, and 'skill' does not exist in type 'Book'.
};
```

**이러한 초과 프로퍼티 검사를 피하려면, 객체에 미리 정의된 객체 변수를 할당하면 됨**

```tsx
let programmingBook: ProgrammingBook = {
  name: "한입 크기로 잘라먹는 타입",
  price: 33000,
  skill: "typescript",
};

book2 = programmingBook;
```

**함수의 인자로 객체를 전달하는 경우에도 마찬가지로 객체가 저장되어있는 변수를 할당해야함**

```tsx
function func(book: Book) {}

func({
  name: "한입 크기로 잘라먹는 리액트",
  price: 33000,
  skill: "reactjs", // Object literal may only specify known properties, and 'skill' does not exist in type 'Book'.
});

func(programmingBook);
```

</aside>

## 대수 타입

---

**여러 개의 타입을 합성해서 새롭게 만들어낸 타입**

→ **합집합 타입**과 **교집합 타입**이 존재

### Union 타입 - 합집합

---

**`|(Bar)` 를 이용해 여러 개의 타입을 포함한 새로운 타입 생성**

**`|(Bar)`**를 이용해 추가할 수 있는 타입의 개수는 무한함

**기본형 타입의 Union 타입**

```tsx
let a: string | number | boolean;
a = 1;
a = "hello";

a = true;

let arr: (string | number | boolean)[] = [1, "hello", true];
```

**객체 타입의 Union 타입**

```tsx
type Dog = {
  name: string;
  color: string;
};

type Person = {
  name: string;
  langauge: string;
};

type Union1 = Dog | Person;
```

→ 합집합에 해당하는 모든 프로퍼티를 가지는 타입을 생성할 수 있음

<aside>
❗

**주의할 점**

타입들을 집합으로 표현했을 때, 모든 합집합 내 어떤 영역에도 포함되지 않는다면 오류 발생

**→ 어떤 타입에도 속할 수 없기 때문**

```tsx
let union1: Union1 = {
  name: "",
  color: "",
};

let union2: Union1 = {
  name: "",
  langauge: "",
};

let union3: Union1 = {
  name: "",
  color: "",
  langauge: "",
};

let union4: Union1 = {
  name: "", // Type '{name: string; }' is not assignable to type 'Union1'
};
```

**name 프로퍼티만 존재하는 객체는 Dog 와 Person 타입 중 어떤 타입에도 해당하지 않음**

**→ 오류 발생!**

</aside>

### Intersection 타입 - 교집합

---

두 타입의 교집에 해당하는, 두 타입을 모두 만족하는 새로운 타입을 생성

```tsx
let variable: number & string;

type Intersection = Dog & Person;

let intersection1: Intersection = {
  name: "",
  color: "",
  langauge: "",
};
```

## 타입 추론

---

명시적으로 타입을 정의해주지 않아도 변수의 초기화 값으로 타입을 추론하는 타입스크립트의 기능

```tsx
let a = 10; // number
let b = "hello"; // string

let c = {
  id: 1,
  name: "김철흥",
  profile: {
    nickname: "definexx",
  },
  urls: ["...."],
};

let { id, name, profile } = c; // id: number, name: string, profile: {}

let [one, two, three] = [1, "hello", true]; // one: number, two: string, three: boolean

function func(message = "hello") { // func: string, message: string
  return "hello";
}
```

**let 변수를 선언하는 경우에 범용적으로 타입 추론이 되는 것을 “타입 넓히기”라고 함**

**변수 선언 시에 초기화 값과 타입에 대한 정의 없이 선언만 할 경우에는, 암묵적 any로 변수가 선언됨**

→ **any 타입**이 어떤 값을 할당하는지에 따라 **“진화”**

```tsx
let d;
d = 10; // any -> number 타입으로 진화
d.toFixed();

d = 'hello'; // any -> string 타입으로 진화
d.toUpperCase();
d.toFixed(); // Property 'toFixed' does not exist on type 'string'.
```

**const 변수를 선언하는 경우에는 리터럴 타입으로 타입이 추론됨**

```tsx
const num = 10; // num: 10
const str = "hello"; // str: "hello"
```

## 타입 단언 (Type assertion)

---

**`as`** 키워드를 사용한 단언식으로 특정 변수의 타입을 단언하는 기능

→ 가능하려면 A as B 형식에서, A는 B의 슈퍼타입이거나 서브타입이여야함

```tsx
type Person = {
  name: string;
  age: number;
};

let person = {} as Person;
person.name = "김철흥";
person.age = 25;

type Dog = {
  name: string;
  color: string;
};

let dog = {
  name: "돌돌이",
  color: "brown",
  breed: "진도",
} as Dog;
```

다중단언으로 단언을 어떻게든 가능하게끔 할 수 있지만,

타입스크립트를 사용하는 의미가 흐려지므로 권장하지 않음

```tsx
let num3 = 10 as string; // 오류 발생
let num3 = 10 as unknown as string; // 오류 발생 X
```

**const 단언**

```tsx
let num4 = 10 as const; // num4: 10

let cat = {
  name: "야옹이", // read-only property
  color: "yellow", // read-only property
} as const;

cat.name = ""; // Cannot assign to 'name' because it is a read-only property.
```

**Non Null 단언**

```tsx
type Post = {
  title: string;
  author?: string;
};

let post: Post = {
  title: "게시글 1",
  author: "김철흥",
}

const len: number = post.author?.length; // Type 'number | undefined' is not assignable to type 'number'.
```

→ 이와 같이 옵셔널 체이닝(**`?.`**)으로 인해 **`len`의 값이** undefined가 될 수도 있으므로

number 타입이 이를 모두 담지 못함

이를 해결하기 위해서 Non null 단언 연산자 사용 (**`!.`**)

```tsx
const len: number = post.author!.length;
```

→ TSC(Typescript Compiler)가 **`!.` 이전에 표기된 값이 undefined 나 null 값이 아니라고 인식**

**이와 같은 타입 단언은 타입을 실제로 변경하는 것이 아닌 TSC(Typescript Compiler)를 조작하는 것!**

## 타입 좁히기

---

**조건문 등을 이용해 넓은 타입에서 좁은 타입으로 타입을 상황에 따라 좁히는 방법**

→ 타입 가드(Type guard)를 사용해 타입 좁히기 가능

1. **`typeof` 사용**

```tsx
function func(value: number | string) {
  if (typeof value === "number") {
    console.log(value.toFixed());
  } else if (typeof value === "string") {
    console.log(value.toUpperCase());
  }
}
```

1. **`instanceof` 사용**
    
    왼쪽에 있는 값이 오른쪽의 인스턴스인지 확인
    
    ```tsx
    function func(value: number | string | Date | null ) {
      if (typeof value === "number") {
        console.log(value.toFixed());
      } else if (typeof value === "string") {
        console.log(value.toUpperCase());
      } else if (value instanceof Date) {
        console.log(value.getTime());
      }
    ```
    

1. **`in` 사용**
    
    ```tsx
    function func(value: number | string | Date | null | Person) {
      if (typeof value === "number") {
        console.log(value.toFixed());
      } else if (typeof value === "string") {
        console.log(value.toUpperCase());
      } else if (value instanceof Date) {
        console.log(value.getTime());
      } else if (value && "age" in value) {
        console.log(`${value.name}은 ${value.age}살 입니다!`);
      }
    }
    ```
    

## 서로소 유니온 타입

---

**교집합이 없는 타입들로만 만든 유니온 타입  (**ex. **`string | number`)**

→ 리터럴 타입을 사용해 각 객체 타입별로 특정 동작을 수행하게끔 코드를 작성할 때, 가독성을 높여줄 수 있음

**Example 1)**

```tsx
type Admin = {
  tag: "ADMIN";
  name: string;
  kickCount: number;
};

type Member = {
  tag: "MEMBER";
  name: string;
  point: number;
};

type Guest = {
  tag: "GUEST";
  name: string;
  visitCount: number;
};

type User = Admin | Member | Guest;

function login(user: User) {
  if(user.tag == "ADMIN") {
    console.log(`${user.name}님 현재까지 ${user.kickCount}명 강퇴했습니다.`);
  } else if(user.tag == "MEMBER") {
    console.log(`${user.name}님 현재까지 ${user.point} 모았습니다.`);
  } else {
    console.log(`${user.name}님 현재까지 ${user.visitCount}번 방문하셨습니다.`)
  }
}
```

**→ tag 프로퍼티의 타입을 문자 리터럴 타입으로 설정함으로써 User 타입은 서로소 유니온 타입이 됨**

**→ 가독성이 좋아지고 코드가 직관적이게 됨**

<aside>
💡

**Tip**

**동시에 여러 상태를 표현해야되는 객체 타입을 정의할 때는,**

**선택적 프로퍼티를 사용하는 것보다는 상태에 따라 여러 타입을 만들고,**

**tag 같은 프로퍼티의 타입으로 문자열 리터럴을 사용해 서로소 유니온 타입으로 만들어주는게 좋음**

**→ 이런 방식으로 구현해야 타입 좁히기 가능**

</aside>

---