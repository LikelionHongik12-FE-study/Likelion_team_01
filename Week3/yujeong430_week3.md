# Section 10~12

# 10. 조건부 타입

```tsx
type A = number extends string ? number : string;
```

참이라면 Number 타입이 되고, 아니라면 String 타입이 됨.

### 제네릭 조건부 타입

```tsx
type StringNumberSwitch<T> = T extends number ? string : number;

let varA: StringNumberSwitch<number>;
// string

let varB: StringNumberSwitch<string>;
// number
```

Number 타입이 할당 → String 타입으로 전환 / String 타입이 할당 → Number 타입으로 전환

공백을 제거하는 함수

```tsx
function removeSpaces<T>(text: T): T extends string ? string : undefined; // T의 값에 따라 반환값의 타입을 정의.
// 조건부 타입의 결과를 함수 내부에서 알 수 있도록 함수 오버로딩 사용.
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

## 분산적인 조건부 타입

조건부 타입의 타입 변수에 Union 타입을 할당하면 분산적인 조건부 타입으로 업그레이드 됨.

```tsx
type StringNumberSwitch<T> = T extends number ? string : number;

(...)

let c: StringNumberSwitch<number | string>;
// string | number
```

**<동작 방식>**

- StringNumberSwitch<number>
- StringNumberSwitch<string>

으로 Union 타입 내부의 모든 타입을 분리하고, 분산된 각 타입의 결과를 모아 다시 Union 타입으로 묶음.

⇒ 결과 : number | string

### Exclude 조건부 타입 구현

```tsx
type Exclude<T, U> = T extends U ? never : T;

type A = Exclude<number | string | boolean, string>;
```

**<동작 방식>**

1. Union 타입이 분리
    - `Exclude<number, string>`
    - `Exclude<string, string>`
    - `Exclude<boolean, string>`
2. 각 분리된 타입을 모두 계산
    - `T = number`, `U = string` 일 때 `number extends string` 은 거짓이므로 결과는 `number`
    - `T = string`, `U = string` 일 때 `string extends string` 은 참이므로 결과는 `never`
    - `T = boolean`, `U = string` 일 때 `boolean extends string` 은 거짓이므로 결과는 `boolean`
3. 계산된 타입들을 모두 Union으로 묶기
    - 결과 : `number | never | boolean`

⇒ never 타입은 공집합이므로, 최종적인 타입 A는 `number | boolean`

## infer

조건부 타입 내에서 특정 타입을 추론하는 문법.

특정 함수 타입에서 반환값의 타입만 추출하는 ReturnType을 만들 때 이용 가능.

```tsx
type ReturnType<T> = T extends () => infer R ? R : never;

type FuncA = () => string;

type FuncB = () => number;

type A = ReturnType<FuncA>;
// string

type B = ReturnType<FuncB>;
// number
```

- `infer R` : 이 조건식을 참이 되도록 만들 수 있는 최적의 R 타입을 추론하라는 뜻.

**<동작 방식>**

1. 타입 변수 T에 함수 타입 FuncA가 할당.
2. T는 () ⇒ string.
3. 조건부 타입의 조건식은 `() ⇒ string extends () ⇒ infer R ? R : never`
4. 조건식을 참으로 만드는 R 타입을 추론 ⇒ string
5. 추론이 가능하면 이 조건식을 참으로 판단. 따라서 결과는 string.
6. 추론이 불가능하다면 조건식을 거짓으로 판단. 결과는 never.

# 11. 유틸리티 타입

타입스크립트가 자체적으로 제공하는 특수한 타입으로, 실무에서 자주 사용되는 유용한 타입을 모아놓은 것.

### Partial<T>

특정 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 변환.

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const draft: Partial<Post> = {
  title: "제목 나중에 짓자",
  content: "초안...",
};
```

**구현**

```tsx
type Partial<T> = {
  [key in keyof T]?: T[key];
};
```

### Required<T>

특정 객체 타입의 모든 프로퍼티를 필수 프로퍼티로 변환.

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

(...)

const withThumbnailPost: Required<Post> = { // ❌ 오류
  title: "한입 타스 후기",
  tags: ["ts"],
  content: "",
  // thumbnailURL: "https://...",
};
```

**구현** 

```tsx
type Required<T> = {
  [key in keyof T]-?: T[key]; // ?를 제거
};
```

### Readonly<T>

모든 프로퍼티를 읽기 전용 프로퍼티로 변환.

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

(...)

const readonlyPost: Readonly<Post> = {
  title: "보호된 게시글입니다.",
  tags: [],
  content: "",
};

readonlyPost.content = '해킹당함'; // ❌
```

**구현**

```tsx
type Readonly<T> = {
  readonly [key in keyof T]: T[key];
};
```

### Pick<T,K>

특정 객체 타입으로부터 특정 프로퍼티만을 골라냄.

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

(...)

const legacyPost: Pick<Post, "title" | "content"> = {
  title: "",
  content: "",
};
// 추출된 타입 : { title : string; content : string }
```

**구현**

```tsx
type Pick<T, K extends keyof T> = {
  [key in K]: T[key];
};
```

### Omit<T,K>

특정 객체 타입으로부터 특정 프로퍼티만을 제거.

```tsx
const noTitlePost: Omit<Post, "title"> = { // title 프로퍼티 제
  content: "",
  tags: [],
  thumbnailURL: "",
};
```

**구현**

```tsx
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

### Record<K,V>

키가 K타입이고, 값이 V타입인 객체 타입을 정의.

```tsx
type Thumbnail = Record<
  "large" | "medium" | "small",
  { url: string }
>;

----------

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
```

**구현**

```tsx
type Record<K extends keyof any, V> = {
  [key in K]: V;
};
```

### Exclude<T,K>

T로부터 U를 제거하는 타입.

```tsx
type A = Exclude<string | boolean, string>;
// boolean
```

**구현**

```tsx
type Exlcude<T, U> = T extends U ? never : T;
```

### Extract<T,K>

T로부터 K를 추출하는 타입.

```tsx
type B = Extract<string | boolean, boolean>;
// boolean
```

**구현**

```tsx
type Extract<T, U> = T extends U ? T : never;
```

### ReturnType<T>

T에 할당된 함수 타입의 반환값을 추출하는 타입.

```tsx
type ReturnType<T extends (...args: any) => any> = T extends (
  ...agrs: any
) => infer R
  ? R
  : never;

function funcA() {
  return "hello";
}

function funcB() {
  return 10;
}

type ReturnA = ReturnType<typeof funcA>;
// string

type ReturnB = ReturnType<typeof funcB>;
// number
```

# 12. 타입스크립트 리액트 시작하기

## 타입스크립트 리액트 시작하기

1. 패키지 설치
    
    `npm i @types/node @types/react @types/react-dom @types/jest`
    
2. `tsconfig.js` 설정
    
    ```json
    {
      "compilerOptions": {
        "target": "ESNext",
        "module": "CommonJS",
        "strict": true,
        "allowJs": true,
        "esModuleInterop": true, // default로 내보내기 값이 없는 모듈에서도 값을 불러올 수 있도록 해줌.
    	  "jsx": "react-jsx" // jsx 해석할 수 있도록
      },
      "include": ["src"]
    }
    ```
    
3. JS 확장자 JSX 로 변경 (TS는 js 파일로부터 JSX 문법 해석 못함)
    
    `App.js`, `index.js` → `App.jsx`, `index.jsx`로 변경
    
    jsx 파일 하나씩 ts파일로 변경해서 타입 검사. 이후 그 안에서 발생하는 오류 해결.
    

`index.jsx`

`document.getElementById("root") as HTMLElement` 값이 null 타입을 반환할 수도 있는데, `createRoot`메소드는 null 타입을 받지 않아서 오류가 발생함.

1. !를 써서 null이 아니라고 단언하기

```tsx
const root = ReactDom.createRoot(document.getElementById("root")!);
```

1. 좀 더 직관적으로 

```tsx
const root = ReactDom.createRoot(
	document.getElementById("root") as HTMLElement
);
```

## 상태관리와 Props1

### useState

```tsx
const [text, setText] = useState(""); // string으로 추론.

const [text, setText] = useState<string>(); // 타입 변수 직접 설정. string과 undefined의 Union 타입으로 추론.
```

초기값으로 전달한 인수의 타입에 따라 state와 state 변화 함수의 타입을 추론함. ⇒ **제네릭 함수.**

`useState()` 로 초기값을 비워둘 경우 undefined로 추론됨.

⇒ 초기값으로 설정할 변수가 마땅히 없을 때는 타입 변수를 직접 설정해 줘야함. 보통은 초기값으로 뭐라도 전달해 주는 것이 좋음.

### event

```tsx
const onChangeInput = (e: React.ChangeEvent>HTMLInputElement>) => {
	setText(e.target.value);
);
```

### todo 객체를 추가

```tsx
// Todo 타입을 정의
interface Todo {
	id: number;
	content: string;
}

...

const [todos, setTodos] = useState<Todo[]>([]);

const idRef = useRef(0); // number로 추론

...

const onClickAdd = () => {
	setTodos([
		...todos,
		{
			id: idRef.current++,
			content: text,
		},
	]);
	setText("");
};

```

### props의 타입 정의하기

컴포넌트로 파일을 분리한다고 할 때 props 전달하는 법

```tsx
...
<Editor onClickAdd={onClickAdd]>
	<div> child </div>
</Editor>
```

```tsx
interface Props {
	onClickAdd: {text: string} => void;
	children : ReactElement;
}

export default function Editor(props: Props){
	...
	const [text, setText] = useState("");
	const onClickButton = () => {
		props.onClickAdd(text);
		setText("");
	};
	...
}
```

## 상태관리와 Props 2

공통으로 여러개의 컴포넌트에서 사용되는 타입을 유지해야 될 때는 별도의 타입스크립트 파일을 만들어서 타입을 분리하는게 좋음.

```tsx
export interfact Todo {
	id: number;
	content: string;
}
```

```tsx
import { Todo } from "../types";

interfact Props extends Todo {
	onClickDelete: {id: number} => void; // onClickDelete 함수는 App.js에 정의되어 있음.
	extra : string; // 새로운 props를 받아야 된다고 할 때
}

export default function TodoItem(props: Props) {
	const onClickButton = () => {
		props.onClickDelete(props.id);
	};
	
	return{
		<div>
			{props.id}번 : {props.content}
		</div>
	};
}
```

### useState를 useReducer로 업그레이드

```tsx
import ...

// Union 타입으로 정의 (create일때와 delete일때)
type Action = 
| {
		type: "CREATE";
		data: {
			id: number;
			content: string;
		};
	}
| { type: "DELETE"; id: number };

function reducer(state: Todo[], action: Action) {
	switch(action.type) {
		case 'CREATE': {
			return [...state, action.data]
		}
		case 'DELETE': {
			return state.filter((it) => it.id !== action.id)
		}
	}
}

function App() {
	const [todos, dispatch] = useReducer(reducer, []);
	
	const idRef = useRef(0);
	
	const onClickAdd = (text: string) => {
		dispatch({
			type : "CREATE",
			data : {
				id: idRef.current++,
				content: text,
			},
		});
	}
	
	const onClickDelete =(id: number) => {
		dispatch({
			type: "DELETE",
			id: id,
		});
	}
}
```

## Context API

두 개의 context를 만들어서 TodoState와 상태 변화 함수들을 나눠서 공급.

```tsx
export const TodoStateContext = React.createContext<Todo[] | null>(null); // todo array거나 null타입임.
export const TodoDispatchContext = React.createContext<{
	onClickAdd : (text : string) => void;
	onClickDelete : (id : number) => void;
} | null>(null);

// 커스텀 훅
export function useTodoDispatch(){]
	const dispatch = useContext(TodoDispatchContext);
	// 타입 좁히기
	if(!dispatch) throw new Error("TodoDispatchContext에 문제가 있다.");
	return dispatch;
}
...
<TodoStateContext.Provider value={todos}>
	<TodoDispatchContext.Provider 
		value={{
			onClickAdd,
			onClickDelete,
		}}
	>
	...
	</TodoDispatchContext.Provider>
</TodoStateContext.Provider>
...
```

```tsx
import ...

// 이제 onClilckAdd를 props로 받지 않음.
interface Props {}

export default function Editor(props: Props){
	const dispatch = useTodoDispatch();
	const [text, setText] = useState("");
	...
	
	const onClickButton = () => {
		dispatch.onClickAdd(text);
		setText("");
	};
	...
}
```

```tsx
import ...

// 이제 onClickDelete를 props로 받지 않음.
interfact Props extends Todo {}

export default function TodoItem(props: Props) {
	const dispatch = useTodoDispatch();
	const onClickButton = () => {
		dispatch.onClickDelete(props.id);
	};
	
	return{
		<div>
			{props.id}번 : {props.content}
			<button onClick = {onClickButton}>삭제</button>
		</div>
	};
}
```

## 외부 라이브러리 사용하기

ts 마크가 붙어있는 라이브러리는 단순히 명령어만 입력해서 바로 사용 가능.

dt 마크가 붙어있는 라이브러리의 경우 추가로 설치해야 할 패키지 있음. ⇒ 타입 정보만 제공하는 패키지가 별도로 있는지 확인해봐야 함.

ex) `@types/react`, `@types/node` … ⇒ **Definitely Types**

## 타입스크립트 템플릿

`npx create-react-app (이름) --template typescript`
