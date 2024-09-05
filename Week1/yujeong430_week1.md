# Section 1 ~ 4

# 타입스크립트 개론

## 타입스크립트란?

자바스크립트를 더 안전하게 사용할 수 있도록 타입 관련 기능들을 추가한 언어.

**JS의 특징**

- 유연한 문법
- 버그 발생 가능성 높음
- 자유로움

→ 웹 브라우저에서만 실행됐던 JS가 Node.js의 등장에 따라 복잡한 대규모 어플리케이션에도 사용되기 시작했고, 유연한 문법이라는 특징이 프로그램의 안정성을 떨어트리는 단점이 됨.

## JS의 단점과 TS의 장점

### 타입시스템

언어의 타입과 관련된 문법 체계.

- **동적 타입 시스템** : 코드를 실행하고 나서 그때 그때 마다 유동적으로 변수의 타입을 결정함.
    - Python, JavaScript
    - 변수의 타입을 미리 결정하지 않음.
    - 코드의 타입 오류를 미리 검사할 수 없기 때문에 런타임에 오류가 발생할 수 있는 치명적인 문제.
- **정적 타입 시스템** : 코드 실행 이전 모든 변수의 타입을 고정적으로 결정.
    - C, JAVA
    - 변수를 선언함과 동시에 타입도 명시.
    - 코드 실행 전에 타입 검사를 모두 마치기 때문에 프로그래머의 의도치 않은 실수를 방지해줌.
    - 작성해야 하는 코드의 양이 늘어남.
- **점진적 타입 시스템** : 타입이 정의된 변수들에 대해서는 타입을 미리 결정하고, 타입이 정의되지 않은 변수들에 대해서는 자동으로 타입을 추론.
    - TypeScript
    - 프로그램 실행 전에 타입을 올바르게 썼는지 검사를 해 타입 안전성을 확보.
    - 변수에 일일이 모두 다 타입을 지정하지 않아도 됨.

## 타입스크립트의 동작 원리

**대다수의 프로그래밍 언어 동작 방식**

![image](https://github.com/user-attachments/assets/ef1fe818-2137-4f2d-8ab8-210c3aa923fe)


코드를 AST 추상 문법 트리로 변환한 후, 이 AST를 바이트코드(컴퓨터가 이해할 수 있는 기계어 같은 형태)로 변환.

**타입스크립트 동작 방식**

![image 1](https://github.com/user-attachments/assets/f8389606-32a2-482c-a25c-1f86f91f374b)


타입스크립트를 AST로 변환하고, 이 AST를 보고 코드상 타입 오류를 검사.

- 타입 오류가 있으면 타입 검사 실패하고 컴파일 중단.
- 타입 오류가 없으면 타입 검사 통과하고 AST를 자바스크립트 코드로 변환.
    
    ⇒ 타입 검사를 통과한 자바스크립트 코드이므로 안전함.
    
    ⇒ 타입스크립트에 작성한 타입 관련 코드들은 이 과정에서 사라짐. 프로그램 실행에 영향X.
    

## 타입스크립트 컴파일러 옵션

`tsconfig.json` 파일에 컴파일의 아주 세부적인 사항들을 설정 가능.

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "outDir": "dist",
    "strict": true,
		"moduleDetection": "force",
		"skipLibCheck": true
  },
  "include": ["src"]
}
```

- **include** : 컴파일 할 타입스크립트 파일의 범위와 위치를 알려줌.
- **target** : 컴파일 결과 생성되는 자바스크립트 코드의 버전을 설정.
- **module** : 변환되는 자바스크립트 코드의 모듈 시스템을 설정.
- **outDir** : 컴파일 결과를 특정 디렉토리에 저장.
- **strict** :  컴파일러 타입 검사 엄격함 수준을 결정.
- **moduleDetection**
    - 타입스크립트의 모든 파일은 기본적으로 전역 파일(모듈). 서로 다른 파일에서 동일한 이름의 변수를 선언하면 오류 발생.
    - export, import 모듈 시스템의 키워드를 하나 이상 활용해서 독립 모듈로 취급되게 만들어야 함.
    - 이걸 자동으로 해주는 옵션이 `"moduleDetection": "force"` . 모든 TS 파일에 export, import 키워드를 추가해줌.

.

# 타입스크립트 기본

## 기본 타입

![image 2](https://github.com/user-attachments/assets/5297b013-bc34-4e80-8009-1438b4d2d5a0)



## 원시 타입

동시에 한개의 값만 저장할 수 있는 타입.

### number 타입

숫자를 의미하는 모든 값을 포함. `toUpperCase` 같은 메소드 사용 x.

```tsx
let num1: number = 123;
let num2: number = -123;
let num3: number = 0.123;
let num4: number = -0.123;
let num5: number = Infinity;
let num6: number = -Infinity;
let num7: number = NaN;
```

### string 타입

```tsx
let str1: string = "hello";
let str2: string = 'hello';
let str3: string = `hello`;
let str4: string = `hello ${str1}`;
```

### boolean 타입

```tsx
let bool1 : boolean = true;
let bool2 : boolean = false;
```

### null 타입

```tsx
let null1: null = null;
```

> 📌 null 타입이 아닌 변수에 임시로 null 값을 할당하고 싶을 때는 `tsconfig.json`에서
`"strictNullChecks": false`  로 설정하면 됨.


### undefined 타입

```tsx
let unde1: undefined = undefined;
```

## 리터럴 타입

딱 하나의 값만 포함하는 타입.

```tsx
let numA: 10 = 10;
let strA: "hello" = "hello";
let boolA: true = true;
let boolB: false = false;
```

## 배열과 튜플

### 배열

```tsx
let numArr: number[] = [1, 2, 3];

let strArr: string[] = ["hello", "Ts"];

let boolArr: Array<boolean> = [true, false, true];  // 제네릭
```

**배열이 다양한 요소를 가질 때 (유니온 타입)**

```tsx
let multiArr: (number | string)[] = [1, "hello"];
```



> 📌 변수 이름에 마우스를 올려서 TS가 타입을 뭐라 추측하는지 확인할 수 있음.


**다차원 배열**

```tsx
let doubleArr : number[][] = [
  [1, 2, 3], 
  [4, 5],
];
```

### 튜플

자바스크립트에 없는 특수한 타입. 길이와 타입이 고정된 배열.

```tsx
let tup1: [number, number] = [1, 2];
tup1 = [1, 2, 3]; // 오류
tup1 = ["1", "2"]; // 오류
```


> 📌 배열 메소드(`push`, `pop` 등)를 사용할 때는 고정된 길이를 무시하고 요소를 추가, 삭제해도 오류가 발생하지 않으므로 주의해야 함.


**튜플을 사용하는 이유**

```tsx
const users: [string, number][] = [
  ["이정환", 1],
  ["이아무개", 2],
  ["김아무개", 3],
  ["박아무개", 4],
  [5, "조아무개"], // 오류 발생
];
```

튜플을 사용하면 인덱스의 위치에 따라서 넣어야 하는 값들이 정해져 있으므로 순서가 중요할 때 형식을 지정해줄 수 있음.

## 객체

**object로 정의**

```tsx
let user = {
  id: 1,
  name: "이정환",
};
```

객체임을 표현할뿐 프로퍼티 정보는 없음. → 프로퍼티에 접근하면 오류 발생.

**객체 리터럴 타입**

```tsx
let user: {
  id: number;
  name: string;
} = {
  id: 1,
  name: "이정환",
};

user.id;
```

객체가 어떻게 생겼는지 객체의 구조를 기준으로 타입을 정의함 ⇒ **구조적 타입 시스템.**

대부분의 언어들은 이름을 기준으로 타입을 정의하는 명목적 타입 시스템.

**선택적 프로퍼티**

```tsx
let user: {
  id?: number; // 선택적 프로퍼티가 된 id
  name: string;
} = {
  id: 1,
  name: "이정환",
};

user = {
  name: "홍길동",
};
```

프로퍼티가 있어도 되고 없어도 됨.

**읽기 전용 프로퍼티**

```tsx
let user: {
  id?: number;
  readonly name: string; // name은 이제 Readonly 프로퍼티가 되었음
} = {
  id: 1,
  name: "이정환",
};

user.name = "dskfd"; // 오류 발생
```

읽기 전용이므로 값을 수정하려고 하면 오류 발생. API Key같은 중요 정보를 보호할 때 사용.

## 타입 별칭과 인덱스 시그니처

### 타입 별칭

공통적으로 적용되는 타입에 대해 코드의 중복을 제거할 수 있음.

`type 타입_이름 = 타입` 형태로 타입을 정의.

```tsx
type User = {
  id: number;
  name: string;
  nickname: string;
  birth: string;
  bio: string;
  location: string;
};

let user: User = {
  id: 1,
  name: "이정환",
  nickname: "winterlood",
  birth: "1997.01.07",
  bio: "안녕하세요",
  location: "부천시",
};

let user2: User = {
  id: 2,
  name: "홍길동",
  nickname: "winterlood",
  birth: "1997.01.07",
  bio: "안녕하세요",
  location: "부천시",
};
```

- 동일한 스코프에 동일한 이름의 타입 별칭을 선언할 수 없음.
- 스코프가 다르면 중복된 이름으로 여러개의 별칭 선언 가능. (해당 스코프 안의 타입으로 인식)

### 인덱스 시그니처

객체 타입을 유연하게 정의할 수 있도록 해줌.

키와 밸류의 타입을 기준으로 규칙을 이용해서 타입을 정의함.

`[key : string] : string` 형태로 정의.

```tsx
type CountryCodes = {
  [key: string]: string;
};

let countryCodes: CountryCodes = {
  Korea: "ko",
  UnitedState: "us",
  UnitedKingdom: "uk",
  // (... 약 100개의 국가)
  Brazil : 'bz'
};
```

📌 인덱스 시그니처를 사용하면서 동시에 추가적인 프로퍼티를 정의할 때는 인덱스 시그니처의 value 타입과 직접 추가한 프로퍼티의 value 타입이 호환되거나 일치해야 함.

```tsx
type CountryNumberCodes = {
  [key: string]: number;
  Korea: string; // 오류!
};
```

## 열거형 타입

자바스크립트에는 존재하지 않고 타입스크립트에서만 사용할 수 있는 특별한 타입.

여러가지 값들에 각각 이름을 부여해 열거해두고 사용하는 타입.

컴파일시 사라지지 않고 자바스크립트 객체로 변환됨.

```tsx
enum Role {
  ADMIN, // 0 할당(자동)
  USER,  // 1 할당(자동)
  GUEST, // 2 할당(자동)
}

const user1 = {
  name: "이정환",
  role: Role.ADMIN, // 0
};

const user2 = {
  name: "홍길동",
  role: Role.USER, // 1
};

const user3 = {
  name: "아무개",
  role: Role.GUEST, // 2
};
```

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

const user1 = {
  name: "이정환",
  role: Role.ADMIN, // 0
  language: Language.korean,// "ko"
};
```

## any와 unknown

### any 타입

타입스크립트에서만 제공되는 타입으로  타입 검사를 받지 않음.

아무 타입의 값이나 범용적으로 담아 사용할 수 있고, 다양한 타입의 메소드 호출 가능.

어떤 타입의 변수던 할당 가능.

```tsx
let anyVar: any = 10;
anyVar = "hello";

anyVar = true;
anyVar = {};

anyVar.toUpperCase();
anyVar.toFixed();
anyVar.a;

let num: number = 10;
num = anyVar;
```



> 📌 any 타입은 타입 검사가 제대로 이루어지지 않기 때문에 위험한 타입. 최대한 사용하지 않는 것이 좋음.


### Unknown 타입

any 타입과 비슷하지만 보다 안전함.

어떤 타입의 값이든 다 저장할 수 있지만 어떤 타입의 변수에도 할당될 수 없음.

연산, 메소드도 사용 불가능.

```tsx
let num: number = 10;
(...)

let unknownVar: unknown;
unknownVar = "";
unknownVar = 1;
unknownVar = () => {};

num = unknownVar; // 오류 !
```

**타입 좁히기** : 조건문으로 특정 값이 특정 타입임을 보장할 수 있게 되면 해당 값의 타입이 자동으로 바뀜.

타입 좁히기를 하면 연산에 참여할 수 있음.

```tsx
if (typeof unknownVar === "number") {
	// 이 조건이 참이된다면 unknownVar는 number 타입으로 볼 수 있음
  unknownVar * 2;
}
```

## void와 never

### void타입

아무런 값도 없음을 의미. 아무 값도 반환하지 않는 함수에서 많이 사용.

```tsx
function func2(): void {
  console.log("hello");
}

```

### never 타입

불가능을 의미. 어떠한 값도 반환할 수 없는 상황일 때 사용.

어떠한 타입의 값도 이 변수에 할당 불가능.

```tsx
function func3(): never {
  while (true) {}
} // 무한 루프이므로 값을 반환하는 것이 불가능.

function func4(): never {
  throw new Error();
} // 의도적으로 오류 발생.
```

# 타입스크립트 이해하기

## 타입은 집합

타입스크립트에서 모드 타입들은 집합으로, 서로 포함하고 포함되는 관계를 가짐.

- **슈퍼 타입** : 다른 타입을 포함하는 타입.
- **서브 타입** : 다른 타입에 포함되는 타입.

![image 3](https://github.com/user-attachments/assets/f11ee9a3-98e4-4c7a-a8c4-bfe9f141d592)


### 타입 호환성

어떤 타입을 다른 타입으로 취급해도 괜찮은지 판단하는 것. 취급되어도 괜찮다면 호환된다.

![image 4](https://github.com/user-attachments/assets/810e9513-4aed-4c6c-8cd6-9bbd77da5dd4)

![image 5](https://github.com/user-attachments/assets/d3f672b1-7c8c-482e-ba23-d7c95e18d9dd)

- **업캐스팅** : 서브타입의 값을 슈퍼타입의 값으로 취급. 모든 상황에서 가능.
- **다운캐스팅** : 슈퍼타입의 갑승ㄹ 서브타입의 갑승로 취급. 대부분의 상황에서 불가능.

## 타입 계층도 살펴보기

![image 6](https://github.com/user-attachments/assets/3fc55db4-5600-4ad9-9ac1-1adafc0a4ab2)

### unknown 타입

- 타입 계층도의 최상단에 위치.
- 모든 타입은 unknown 타입으로 업캐스팅 가능. 모든 타입의 슈퍼타입.
- unknown 타입의 값은 any를 제외한 어떤 타입의 변수에도 할당 불가능.

### never 타입

- 타입 계층도의 최하단에 위치.
- never 타입은 모든 타입으로 업캐스팅 가능. 모든 타입의 서브타입.
- 그 어떤 타입도 never 타입에 할당될 수 없음.

### void 타입

- undefined의 슈퍼타입 → void로 선언한 함수에서 undefined 반환 가능 (업캐스팅).
- void 타입에는 undefined, never 이외에 다른 값을 할당할 수 없음.

### any 타입

- 모든 타입의 슈퍼타입이 될 수도 있고 서브타입이 될 수도 있음.
- 모든 타입으로 업캐스팅, 다운캐스팅 가능.

## 객체 타입의 호환성

모든 객체 타입도 다른 객체 타입과 슈퍼-서브타입 관계를 가지므로, 업캐스팅은 가능하고 다운캐스팅은 불가능함.

```tsx
type Animal = { // Dog 타입의 슈퍼타입. name, color를 가지고 있는 객체들을 모두 포함하는 집합이므로.
  name: string;
  color: string;
};

type Dog = { // name, color에 추가로 breed까지 가져야 함.
  name: string;
  color: string;
  breed: string;
};

let animal: Animal = {
  name: "기린",
  color: "yellow",
};

let dog: Dog = {
  name: "돌돌이",
  color: "brown",
  breed: "진도",
};

animal = dog; // ✅ OK
dog = animal; // ❌ NO
```

### 초과 프로퍼티 검사

타입에 정의된 프로퍼티 외에 초과된 프로퍼티를 갖는 객체를 변수에 할당할 수 없음.

객체 리터럴을 사용하지 않으면 발생하지 않음.

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

(...)

let book2: Book = { // 오류 발생
  name: "한 입 크기로 잘라먹는 리액트",
  price: 33000,
  skill: "reactjs",
};

let book3: Book = programmingBook; // 앞서 만들어둔 변수
```

## 대수 타입

### 합집합(Union) 타입

```tsx
// 변수
let a: string | number;

a = 1;
a = "hello";

// 배열
let arr: (number | string | boolean)[] = [1, "hello", true];

// 객체
type Dog = {
  name: string;
  color: string;
};

type Person = {
  name: string;
  language: string;
};

type Union1 = Dog | Person;
```

### 교집합(Intersection) 타입

```tsx
// 변수
let variable: number & string; // never 타입으로 추론

// 객체
type Dog = {
  name: string;
  color: string;
};

type Person = {
  name: string;
  language: string;
};

type Intersection = Dog & Person;

let intersection1: Intersection = {
  name: "",
  color: "",
  language: "",
};
```

## 타입 추론

 타입이 정의되어 있지 않는 변수의 타입을 타입스크립트가 자동으로 추론.

**타입 넓히기** : 프로그래머가 범용적으로 변수를 사용할 수 있도록 타입스크립트가 조금 더 넓은 타입으로 추론해주는 과정.

ex) `let a = 10;` 으로 선언하면 a를 number 리터럴이 아니라 number 타입으로 추론.

### 타입 추론이 가능한 상황

1. **변수 선언**
    1. 초기값을 기준으로 타입 추론
2. **구조 분해 할당**
3. **함수의 반환값**
    1. return 문을 기준으로 추론.
4. **기본값이 설정된 매개변수**
    1. 기본값을 기준으로 추론.
    

### 주의해야 할 상황들

1. **암시적으로 any 타입으로 추론**
    1. 변수 초기값을 생략하면 any 타입으로 추론.
    2. 이후 변수에 값을 할당하면 그 다음 라인부터 해당 값의 타입으로 변화. 코드의 타입에 따라 타입이 계속 변화함. ⇒ **any의 진화**.
        
        ```tsx
        let d;
        d = 10;
        d.toFixed();
        
        d = "hello";
        d.toUpperCase();
        d.toFixed(); // 오류 
        ```
        
2. **const 상수의 추론**
    1. 상수는 설정한 값을 변경할 수 없기 때문에 가장 좁은 타입으로 추론.
        
        ```tsx
        const num = 10;
        // 10 Number Literal 타입으로 추론
        
        const str = "hello";
        // "hello" String Literal 타입으로 추론
        ```
        

### 최적 공통 타입

다양한 타입의 요소를 담은 배열을 변수의 초기값으로 설정하면 최적의 공통 타입으로 추론.

## 타입 단언

`값 as 타입` 으로 특정 값을 원하는 타입으로 단언 가능.

초기화 할 때는 빈 객체를 넣어두고 싶을 때 사용. 해당 타입이라고 타입스크립트에게 단언만 해주면 됨.

초과 프로퍼티 검사를 피할때도 사용.

```tsx
type Person = {
  name: string;
  age: number;
};

let person = {} as Person;
person.name = "";
person.age = 23; 

type Dog = {
  name: string;
  color: string;
};

let dog: Dog = {
  name: "돌돌이",
  color: "brown",
  breed: "진도",
} as Dog
```

### 타입 단언의 조건

**`A as B`로 표현했을 때**

- A가 B의 슈퍼타입
- A가 B의 서브타입

겹치는 부분이 조금이라도 있어야 허용을 해줌.

```tsx
let num1 = 10 as never;   // ✅ num은 never의 슈퍼타입
let num2 = 10 as unknown; // ✅ num은 unknown의 서브타입

let num3 = 10 as string;  // ❌ 슈퍼-서브타입 x
```

### 다중 단언

다중 단언을 사용해 불가능했던 단언을 가능하게 만들 수 있음. 

그러나 슈퍼-서브 관계를 갖지 않는 타입을 단언할 경우 오류가 발생할 확률이 높아지므로 사용하지 않는 것이 좋음. 

```tsx
let num3 = 10 as unknown as string;
// num을 unknown으로 단언 -> unknown을 string으로 단언
```

### const 단언

const 타입으로 단언하면 변수를 const로 선언한 것과 비슷하게 타입 변경됨.

```tsx
let num4 = 10 as const;
// 10 Number Literal 타입으로 단언됨

let cat = {
  name: "야옹이",
  color: "yellow",
} as const;
// 모든 프로퍼티가 readonly를 갖도록 단언됨
```

### Non Null 단언

값 뒤에 !를 붙여 값이 undefined나 null이 아닐 것으로 타입스크립트가 믿게 함.

```tsx
type Post = {
  title: string;
  author?: string;
};

let post: Post = {
  title: "게시글1",
};

const len: number = post.author!.length;
```

## 타입 좁히기

조건문을 통해 변수가 특정 타입임을 보장하면 해당 조건문 내부에서는 변수의 타입이 보장된 타입으로 좁혀짐. 

```tsx
function func(value: number | string) {
  if (typeof value === "number") { // 타입을 좁히는 표현 : 타입 가드
    console.log(value.toFixed());
  } else if (typeof value === "string") {
    console.log(value.toUpperCase());
  }
}
```

### instanceof 타입 가드

내장 클래스 타입을 보장할 수 있는 타입가드를 만들 수 있음.

```tsx
function func(value: number | string | Date | null) {
  if (typeof value === "number") {
    console.log(value.toFixed());
  } else if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else if (value instanceof Date) {
    console.log(value.getTime());
  }
}
```

### in 타입 가드

직접 만든 타입과 함께 사용할 때.

```tsx
type Person = {
  name: string;
  age: number;
};

function func(value: number | string | Date | null | Person) {
  if (typeof value === "number") {
    console.log(value.toFixed());
  } else if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else if (value instanceof Date) {
    console.log(value.getTime());
  } else if (value && "age" in value) {
    console.log(`${value.name}은 ${value.age}살 입니다`)
  }
}
```

## 서로소 유니온 타입

서로소 관계에 있는 타입들을 모아 만든 유니온 타입.

```tsx
// 회원 유형
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
  if (user.tag === "ADMIN") {
    console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
  } else if (user.tag === "MEMBER") {
    console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
  } else {
    console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
  }
```



> 📌 동시에 여러가지 상태를 표현해야 되는 객체 타입을 정의할 때는 선택적 프로퍼티를 사용하는 것 보다 상태에 따라 타입들을 잘게 쪼개고 state, tag 같은 프로퍼티들을 추가해 서로소 유니온 타입으로 만들어주는게 안전함.

