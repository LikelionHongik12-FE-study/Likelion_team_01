# Section 10~12

---

## 조건부 타입

---

**`extends`**와 삼항 연산자를 이용해 조건에 따라 타입을 다르게 정의하는 문법

```tsx
type A = number extends string ? number : string;
```

**→ `T extends U`의 조건문은 T가 U와 호환됨을 판단하면 됨 (T가 U의 부분집합인지의 여부)**

**조건부 타입은 제네릭과 함께 사용할 때 활용도가 높음**

```tsx
type StringNumberSwitch<T> = T extends number ? string : number;

let varA: StringNumberSwitch<number>;
// string

let varB: StringNumberSwitch<string>;
// number
```

**Example)**

```tsx
// 매개변수로 String 타입의 값을 제공받아 공백을 제거하는 함수
function removeSpaces(text: string) {
  return text.replaceAll(" ", "");
}

let result = removeSpaces("hi im winterlood");
```

만약, **`removeSpaces` 함수가 인수로 undefined 나 null 값을 가질 수 있다고 하면**

```tsx
function removeSpaces(text: string | undefined | null) {
  if (typeof text === "string") {
    return text.replaceAll(" ", "");
  } else {
    return undefined;
  }
} 

let result = removeSpaces("hi im winterlood");
// string/undefined
```

→ 위와 같이 조건문을 활용해 타입을 좁혀 사용해야함

→ 하지만, 이 경우에는 **`result`** 의 타입이 **`string | undefined` 이 되므로 값을 사용하는데 제약이 있음**

```tsx
function removeSpaces<T>(text: T): T extends string ? string : undefined {
  if (typeof text === "string") {
    return text.replaceAll(" ", ""); // ❌
  } else {
    return undefined; // ❌
  }
} 

let result = removeSpaces("hi im winterlood");
// string

let result2 = removeSpaces(undefined);
// undefined
```

→ 이 경우에는 조건문 안의 리턴값을 함수 내부에서 알 수 없음

→ 함수 오버로딩을 이용하면, 오버로드 시그니처의 조건부 타입을 구현 시그니쳐 내부에서 추론할 수 있음

```tsx
function removeSpaces<T>(text: T): T extends string ? string : undefined;
function removeSpaces(text: any) {
  if (typeof text === "string") {
    return text.replaceAll(" ", "");
  } else {
    return undefined;
  }
}

let result = removeSpaces("hi im winterlood");
// string

let result2 = removeSpaces(undefined);
// undefined
```

### 분산적인 조건부 타입

---

조건부 타입의 타입 변수에 Union 타입을 할당하면 분산적인 조건부 타입으로 인식

→ 타입 변수에 할당된 모든 Union 타입이 분리됨

**Example)**

```tsx
type StringNumberSwitch<T> = T extends number ? string : number;

let c: StringNumberSwitch<number | string>;
// Union 타입으로 동작할 경우: number 
// 분산적인 조건부 타입으로 동작하는 경우: string | number
```

**`Exclude` 조건부 타입**

---

Union 타입으로부터 특정 타입만 제거하는 타입

```tsx
type Exclude<T, U> = T extends U ? never : T;

type A = Exclude<number | string | boolean, string>; // number | boolean
```

→ **`never` 타입은 공집합을 의미하므로, Union으로 묶일 경우 사라짐**

### infer

---

조건부 타입 내에서 특정 타입을 추론하는 문법

**Example)**

**ReturnType (특정 함수 타입에서 반환값의 타입만 추출하는 특수한 조건부 타입)**

```tsx
type ReturnType<T> = T extends () => infer R ? R : never;

type FuncA = () => string;

type FuncB = () => number;

type A = ReturnType<FuncA>;
// string

type B = ReturnType<FuncB>;
// number
```

**`T extends () => infer R`**에서 **`infer R`**은

이 조건식을 참이 되도록 만들 수 있는 **최적의 R 타입을 추론하라는 의미**

```tsx
type C = ReturnType<number>;
// 조건식을 만족하는 R추론 불가능
// never
```

→ 이 경우에는 number 타입과 함수 타입 간의 최적의 R을 추론하는 것이 불가능

→ 조건문을 거짓으로 판단, **`C`** 는 never 타입이 됨

**Example) Promise의 resolve 타입을 infer를 이용해 추출하는 예시**

```tsx
type PromiseUnpack<T> = T extends Promise<infer R> ? R : never;

type PromiseA = PromiseUnpack<Promise<number>>;

type PromiseB = PromiseUnpack<Promise<string>>;
```

## 유틸리티 타입

---

타입스크립트가 자체적으로 제공하는 특수한 타입

→ 타입 조작 기능(mapped 타입, 제네릭 타입, 조건부 타입) 이용해 실무에서 자주 사용되는 유용한 타입

### Mapped 타입 기반

---

### Partial<T>

---

특정 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 바꿔주는 타입

**Example)**

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const draft: Post = { // ❌ tags 프로퍼티가 없음
  title: "제목은 나중에 짓자...",
  content: "초안...",
};
```

→ 임시저장의 경우에는 모든 프로퍼티들이 필수적이지 않음

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

// 직접 구현한 Partial 타입
type Partial<T> = {
  [key in keyof T]?: T[key];
};

const draft: Partial<Post> = {
  title: "제목 나중에 짓자",
  content: "초안...",
};
```

### Required <T>

---

특정 객체 타입의 모든 프로퍼티를 필수 프로퍼티로 변환하는 타입

**Example)**

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const withThumbnailPost: Required<Post> = { // 오류 발생
  title: "한입 타스 후기",
  tags: ["ts"],
  content: "",
  // thumbnailURL: "https://...",
};

// 직접 구현한 Required 타입
type Required<T> = {
  [key in keyof T]-?: T[key];
};
```

### Readonly<T>

---

특정 객체 타입에서 모든 프로퍼티를 읽기 전용 프로퍼티로 만들어주는 타입

**Example)**

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const readonlyPost: Readonly<Post> = {
  title: "보호된 게시글입니다.",
  tags: [],
  content: "",
};

readonlyPost.content = '해킹당함'; // 수정 불가능

// 직접 구현한 Readonly 타입
type Readonly<T> = {
  readonly [key in keyof T]: T[key];
};
```

### Pick<T, K>

---

객체 타입으로부터 특정 프로퍼티만 골라내는 타입

**Example)**

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const legacyPost: Pick<Post, "title" | "content"> = {
  title: "",
  content: "",
};
// 추출된 타입 : { title : string; content : string }

// 직접 구현한 Pick 타입
type Pick<T, K extends keyof T> = {
  [key in K]: T[key];
};
```

→  K가 T의 key로만 이루어진 String Literal Union 타입임을 보장해 주어야함

### Omit<T, K>

---

객체 타입으로부터 특정 프로퍼티를 제거하는 타입

**Example)**

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const noTitlePost: Post = { // ❌
  content: "",
  tags: [],
  thumbnailURL: "",
};

const noTitlePost: Omit<Post, "title"> = {
  content: "",
  tags: [],
  thumbnailURL: "",
};

// 직접 구현한 Omit 타입
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

### Record<K,V>

---

객체의 키와 값의 타입을 정의할 수 있는 유틸리티 타입

→ 같은 구조의 프로퍼티들을 타입에 추가해야할 때 유용

**Example)**

```tsx
type Thumbnail = {
  large: {
    url: string;
  };
  medium: {
    url: string;
  };
  small: {
    url: string;
  };
};

type Thumbnail = {
 (...)
  watch: {
    url: string;
  };
};
```

→ 같은 구조의 프로퍼티가 추가되어야할 때, Record 타입을 사용해 코드 중복 방지

```tsx
type Thumbnail = Record<
  "large" | "medium" | "small",
  { url: string }
>;

// 직접 구현한 Record 타입
type Record<K extends keyof any, V> = {
  [key in K]: V;
};
```

## 리액트와 타입스크립트

---

### 타입스크립트 리액트 시작하기

---

1. 타입 선언 패키지 추가

2. tsconfig.json 파일 생성

3. js 확장자를 jsx 파일로 변환

4. jsx 확장자를 tsx로 바꾸고, 발생하는 오류들을 순차적으로 해결

### useState 사용하기

---

useState 내 Setter 함수는 제네릭 함수

→ 초기 값으로 타입을 추론

→ 초기 값이 없다면 제네릭으로 타입을 정의해줄 수 있지만, undefined 와의 유니온 타입으로 추론됨

**→ 이러한 이유로 초기 값을 사용하는 것을 권장**

```tsx
import { useState } from 'react';

function App() { 
	const [text, setText] = useState("");
}
```