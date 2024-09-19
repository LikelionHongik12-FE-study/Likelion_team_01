- 한 입 크기로 잘라먹는 타입스크립트(TypeScript)
	- 섹션 10. 조건부 타입
		- 섹션 10. 조건부 타입
			- 조건부 타입 소개
				- 조건부 타입 소개
				    - 조건부 타입은 JavaScript의 삼항 연산자(`조건 ? 참일 때 : 거짓일 때`)를 이용
					    - 조건에 따라 타입을 결정하는 문법
						    ```typescript
						    type A = NumberType extends StringType ? TrueType : FalseType;
						    ```
							- 위 예제에서 `NumberType`이 `StringType`을 확장하면 `A`는 `TrueType`이 됨
								- 그렇지 않으면 `A`는 `FalseType`이 됨
						- 기본적인 조건부 타입 사용
							```typescript
							type A = number extends string ? string : number;
							// 결과: A는 number 타입
							```
							- `number` 타입은 `string` 타입을 확장하지 않기 때문에
								- 이 조건은 거짓
								- `A`는 `number` 타입이 됨
						- 객체 타입과 조건부 타입
							```typescript
							type ObjectA = { a: number };
							type ObjectB = { a: number; b: number };
							
							type B = ObjectB extends ObjectA ? number : string;
							// 결과: B는 number 타입
							```
							- `ObjectB`는 `ObjectA`를 확장
								- 조건이 참이 되어 `B`는 `number` 타입이 됨
						- 조건부 타입은 제네릭과 함께 사용할 때 더욱 강력해짐
							- 조건부 타입과 제네릭의 결합
								```typescript
								type StringNumberSwitch<T> = T extends number ? string : number;
								
								let varA: StringNumberSwitch<number>; // varA의 타입은 string
								let varB: StringNumberSwitch<string>; // varB의 타입은 number
								```
								- 위 예제에서 `T`에 따라 `StringNumberSwitch`의 결과 타입이 달라짐
								- `T`가 `number`이면 `string` 타입
									- 그렇지 않으면 `number` 타입을 반환
						- 함수의 반환 타입 결정
							- 조건부 타입은 함수의 반환 타입을 입력 값에 따라 동적으로 결정할 때 유용
							- 공백을 제거하는 함수
							  ```typescript
							  function removeSpaces<T>(text: T): T extends string ? string : undefined {
							    if (typeof text === 'string') {
							      return text.replaceAll(' ', '') as any;
							    } else {
							      return undefined as any;
							    }
							  }
							
							  let result = removeSpaces('Hi I\'m TypeScript');
							  // result의 타입은 string
							
							  let result2 = removeSpaces(undefined);
							  // result2의 타입은 undefined
				  ```
							  - 위 함수에서 입력 값 `text`가 문자열이면 공백이 제거된 문자열을 반환
								  - 그렇지 않으면 `undefined`를 반환
									  - 반환 타입을 조건부 타입으로 정의
										  - 입력 타입에 따라 정확한 반환 타입을 지정 가능
						- 함수 내부의 타입 오류 해결
							- 하지만 함수 내부에서는 제네릭 타입 `T`의 구체적인 타입을 알 수 없음
								- 타입스크립트는 조건부 타입의 결과를 추론하지 못하고 오류를 발생시킬 수 있음
									- 이를 해결하기 위해 함수 오버로딩을 사용
						- 함수 오버로딩 적용
							```typescript
							function removeSpaces(text: string): string;
							function removeSpaces(text: undefined): undefined;
							function removeSpaces(text: any): any {
							if (typeof text === 'string') {
							  return text.replaceAll(' ', '');
							} else {
							  return undefined;
							}
							}
							```
						  - 함수 오버로딩을 통해 입력 값의 타입에 따라 반환 타입을 명확하게 지정
							  - 구현부에서는 `any` 타입을 사용하여 타입 오류를 방지
					- 조건부 타입은 타입스크립트에서 조건에 따라 타입을 결정하는 강력한 기능
						- 제네릭과 함께 사용하면 타입을 더욱 유연하고 강력하게 제어 가능
					- 함수 오버로딩을 통해 조건부 타입의 제한 사항을 극복
						- 입력 값에 따라 정확한 반환 타입을 지정 가능
					- 조건부 타입은 복잡한 타입 변환이나 타입 추론을 처리할 때 매우 유용
				
			- 분산적인 조건부 타입
				- 분산적인 조건부 타입
					- 조건부 타입의 복습
					    - 조건부 타입은 제네릭과 함께 사용하여 타입 변수 `T`에 따라 다른 타입을 결정
						    ```typescript
						    type StringNumberSwitch<T> = T extends number ? string : number;
						    ```
					    - `T`가 `number`이면 `string` 타입을 반환
						    - 그렇지 않으면 `number` 타입을 반환
								```typescript
								let a: StringNumberSwitch<number>; // a의 타입은 string
								let b: StringNumberSwitch<string>; // b의 타입은 number
								```
						  - 분산적인 조건부 타입이란?
						    - 유니온 타입을 조건부 타입의 타입 변수로 전달할 때 발생하는 특별한 동작
							    ```typescript
							    type C = StringNumberSwitch<number | string>;
							    // 결과: C는 string | number 타입
							    ```
						    - 조건부 타입에 유니온 타입이 들어가면 각 구성 요소에 조건부 타입을 분산 적용
							    - 결과를 유니온으로 합침
							- 여러 타입의 분산적인 조건부 타입 예제
							    ```typescript
							    let d: StringNumberSwitch<boolean | number | string>;
							    // d의 타입은 number | string
							    ```
							- 각 타입에 조건부 타입 적용 후 결과를 유니온으로 합침
						- 특정 타입 제거하기 (Exclude)
						    - 분산적인 조건부 타입을 이용하여 유니온 타입에서 특정 타입을 제외
							    ```typescript
							    type Exclude<T, U> = T extends U ? never : T;
							
							    type A = Exclude<number | string | boolean, string>;
							    // A의 타입은 number | boolean
							    ```
						- 특정 타입 추출하기 (Extract)
							- 특정 타입만 추출
							    ```typescript
							    type Extract<T, U> = T extends U ? T : never;
							
							    type B = Extract<number | string | boolean, string>;
							    // B의 타입은 string
							    ```
						- 분산적인 조건부 타입 방지하기
							- 분산적인 동작을 원치 않을 때는 조건부 타입의 양쪽을 튜플로 감쌈
								```typescript
								type NonDistributed<T> = [T] extends [U] ? TrueType : FalseType;
								```
							    - 이렇게 하면 유니온 타입이 분리되지 않고 전체 타입으로서 조건부 타입이 적용
								```typescript
								type Test = [number | string] extends [number] ? true : false;
								// Test의 값은 false
								```
				    - 분산적인 조건부 타입은 유니온 타입의 각 구성 요소에 조건부 타입을 적용하고 결과를 유니온으로 합치는 것
				    - `Exclude`, `Extract` 같은 유틸리티 타입을 구현할 때 유용
				    - 분산적인 동작을 막고 싶을 때는 조건부 타입의 양쪽을 튜플로 감싸면 됨
				
			- infer - 조건부 타입 내에서 타입 추론하기
				- infer - 조건부 타입 내에서 타입 추론하기
					- `infer`는 조건부 타입 내에서 특정 타입을 추론할 때 사용하는 특별한 키워드
					- 이를 통해 타입스크립트는 조건부 타입의 문맥에서 원하는 타입을 추출할 수 있음
						
					- 함수의 반환 타입 추출하기
						- 함수 타입에서 반환 타입만 추출하는 방법
						    ```typescript
						    type Func = () => string;
						    ```
						  - 일반적인 조건부 타입 사용
						    ```typescript
						    type ReturnType<T> = T extends () => string ? string : never;
						
						    type A = ReturnType<Func>; // 결과: string
						    ```
						    - 이 방법은 반환 타입이 `string`으로 고정되어 있어 다른 반환 타입의 함수에는 적용 불가
						- `infer` 키워드 사용하기
						    ```typescript
						    type ReturnType<T> = T extends () => infer R ? R : never;
						
						    type A = ReturnType<() => string>; // 결과: string
						    type B = ReturnType<() => number>; // 결과: number
								    ```
						    - `infer R`을 통해 함수의 반환 타입을 동적으로 추론
						    - 어떤 함수 타입이 오더라도 그 함수의 반환 타입을 추출 가능
						- `infer`의 동작 원리
							- 조건부 타입의 참이 되도록 `R`을 추론
							- 추론이 가능하면 조건식은 참이 되고, 추론된 타입 `R`을 반환
							- 추론이 불가능하면 조건식은 거짓이 되어 `never` 타입을 반환
							    ```typescript
							    type ReturnType<T> = T extends () => infer R ? R : never;
							
							    type C = ReturnType<number>; // 결과: never
							    ```
								- `number` 타입은 함수 타입이 아니므로 추론 불가, 결과는 `never`
						- `infer`를 활용한 실용적인 예제
							- Promise의 결과 타입 추출
								- `Promise` 타입에서 내부의 결과 타입을 추출
								    ```typescript
								    type PromiseUnpacked<T> = T extends Promise<infer R> ? R : never;
								
								    type PromiseA = PromiseUnpacked<Promise<number>>; // 결과: number
								    type PromiseB = PromiseUnpacked<Promise<string>>; // 결과: string
								    ```
								- 구현 방법
								    - 조건부 타입 작성
								      ```typescript
								      type PromiseUnpacked<T> = T extends Promise<infer R> ? R : never;
								      ```
									- 동작 원리
										- `T`가 `Promise<infer R>`를 확장하는지 검사
										- 확장한다면 `infer R`로 `Promise` 내부의 타입을 추론하여 `R`에 할당
										- 추론된 `R`을 반환
					- `infer` 키워드는 조건부 타입 내에서 타입 변수를 선언하고 특정 위치의 타입을 추론
						- 일반화된 타입 추출이 가능하며, 다양한 상황에서 활용 가능
					- 주의점
						- `infer`를 사용할 때 조건식이 거짓이면 추론이 불가능하므로 `never` 타입이 반환됨
					- `infer` 키워드를 사용하면 타입스크립트에서 타입 추론을 더욱 유연하고 강력하게 활용할 수 있음
					- 함수의 반환 타입, Promise의 결과 타입 등에서 유용하게 타입 추출 가능
				
		
	- 섹션 11. 유틸리티 타입
		- 섹션 11. 유틸리티 타입
			- 유틸리티 타입 소개
				- 유틸리티 타입 소개
					- 유틸리티 타입은 TypeScript가 제공하는 특수한 타입
						- 제네릭, 맵드 타입, 조건부 타입 등의 기능을 활용하여 타입을 변환하거나 조작하는 데 사용
					- 실무에서 자주 사용하는 타입 변환 패턴을 미리 구현해 놓은 것
					
					- 주요 유틸리티 타입 소개
						- `Readonly<T>`
						    - 주어진 타입 `T`의 모든 프로퍼티를 읽기 전용으로 만듦
						      ```typescript
						      type Person = {
						        name: string;
						        age: number;
						      };
						
						      type ReadonlyPerson = Readonly<Person>;
						
						      // ReadonlyPerson 타입은 다음과 같습니다:
						      // {
						      //   readonly name: string;
						      //   readonly age: number;
						      // }
						      ```
							    - `ReadonlyPerson` 타입의 프로퍼티는 수정할 수 없으며, 수정하려고 하면 컴파일 오류 발생
						- `Partial<T>`
							- 타입 `T`의 모든 프로퍼티를 선택적(`optional`)으로 만듦
						      ```typescript
						      type Person = {
						        name: string;
						        age: number;
						      };
						
						      type PartialPerson = Partial<Person>;
						
						      // PartialPerson 타입은 다음과 같습니다:
						      // {
						      //   name?: string;
						      //   age?: number;
						      // }
						      ```
							    - `PartialPerson` 타입의 프로퍼티는 없어도 되며, 객체를 생성할 때 일부 프로퍼티만 지정 가능
						- 다양한 유틸리티 타입
							- TypeScript는 이 외에도 다양한 유틸리티 타입을 제공
								- 맵드 타입 기반 유틸리티 타입 (6가지)
									- `Readonly`
									- `Partial`
									- `Required`
									- `Record`
									- `Pick`
									- `Omit`
								- 조건부 타입 기반 유틸리티 타입 (3가지)
									- `Exclude`
									- `Extract`
									- `NonNullable`
						- 유틸리티 타입 직접 만들어보기
				
			- 맵드 타입 기반의 유틸리티 타입 1- Partial, Required, Readonly
				- 맵드 타입 기반의 유틸리티 타입 1 - Partial, Required, Readonly
					- TypeScript의 유틸리티 타입 중 Mapped Type을 기반으로 만들어진 Partial, Required, Readonly 타입
					
					- Partial 타입
						- `Partial<T>`는 주어진 객체 타입 `T`의 모든 프로퍼티를 선택적(optional) 프로퍼티로 변환하는 유틸리티 타입
						    ```typescript
						    interface Post {
						      title: string;
						      tags: string[];
						      content: string;
						      thumbnailUrl?: string;
						    }
						
						    const draft: Partial<Post> = {
						      title: "제목 나중에 짓자",
						      content: "초안",
						    };
				    ```
						    - `Partial<Post>`를 사용하여 `draft` 객체를 선언
							    - 모든 프로퍼티가 선택적이므로 일부 프로퍼티만 초기화해도 오류가 발생하지 않음
						    ```typescript
						    type Partial<T> = {
						      [Key in keyof T]?: T[Key];
						    };
						    ```
							- `keyof T`
								- 타입 `T`의 모든 키를 가져옴
							- `[Key in keyof T]`
								- 맵드 타입을 사용하여 `T`의 모든 키를 순회
							- `?:`
								- 각 프로퍼티를 선택적으로 만듦
							- `T[Key]`
								- 원래 타입의 프로퍼티 타입을 그대로 사용
					- Required 타입
						- `Required<T>`는 주어진 객체 타입 `T`의 모든 프로퍼티를 필수(required) 프로퍼티로 변환하는 유틸리티 타입
						    ```typescript
						    const withThumbnailPost: Required<Post> = {
						      title: "TypeScript 강의 후기",
						      tags: ["TypeScript", "강의"],
						      content: "내용",
						      thumbnailUrl: "https://example.com/thumbnail.jpg",
						    };
						    ```
							- `Required<Post>`를 사용하면 `thumbnailUrl`도 필수 프로퍼티가 되어 반드시 초기화해야 함
								- 그렇지 않으면 컴파일 오류가 발생
							```typescript
							type Required<T> = {
							  [Key in keyof T]-?: T[Key];
							};
							```
							- `-?`
								- 프로퍼티의 선택적 속성을 제거하여 필수로 만듦
					- Readonly 타입
						- `Readonly<T>`는 주어진 객체 타입 `T`의 모든 프로퍼티를 읽기 전용(readonly) 프로퍼티로 변환하는 유틸리티 타입
						    ```typescript
						    const readonlyPost: Readonly<Post> = {
						      title: "보호된 게시글",
						      tags: [],
						      content: "",
						    };
						
						    // readonlyPost.title = "새로운 제목"; // 오류 발생: 읽기 전용 프로퍼티이므로 수정 불가
						    ```
							- `Readonly<Post>`를 사용하면 모든 프로퍼티가 읽기 전용이 됨
								- 값을 수정하려고 하면 컴파일 오류가 발생
								```typescript
								type Readonly<T> = {
								  readonly [Key in keyof T]: T[Key];
								};
								```
								- `readonly`
									- 각 프로퍼티를 읽기 전용으로 만듦
			- 맵드 타입 기반의 유틸리티 타입 2- Pick, Omit, Record
				- 맵드 타입 기반의 유틸리티 타입 2 - Pick, Omit, Record
					- Mapped Type을 기반으로 제작된 유틸리티 타입인 Pick, Omit, Record 타입
					
					- Pick 타입
						- `Pick<T, K>`는 객체 타입 `T`에서 특정 프로퍼티 `K`만 선택하여 새로운 타입을 만드는 유틸리티 타입
						    ```typescript
						    interface Post {
						      title: string;
						      tags: string[];
						      content: string;
						      thumbnailUrl?: string;
						    }
						
						    const legacyPost: Pick<Post, "title" | "content"> = {
						      title: "옛날 글",
						      content: "옛날 콘텐츠",
						    };
						    ```
						- `Post` 인터페이스에서 `"title"`과 `"content"` 프로퍼티만 선택하여 `legacyPost` 타입을 정의
							- `tags`나 `thumbnailUrl` 없이도 객체를 생성 가능
								- 직접 구현
									```typescript
									type Pick<T, K extends keyof T> = {
									  [Key in K]: T[Key];
									};
									```
							      - `K extends keyof T`
								      - `K`는 `T`의 키들 중 하나여야 함
							      - `[Key in K]`
								      - `K`에 있는 키들만 순회
							      - `T[Key]`
								      - 원본 타입 `T`에서 해당 키의 타입을 가져옴
					- Omit 타입
						- `Omit<T, K>`는 객체 타입 `T`에서 특정 프로퍼티 `K`를 제외하고 새로운 타입을 만드는 유틸리티 타입
						    ```typescript
						    const noTitlePost: Omit<Post, "title"> = {
						      tags: ["TypeScript"],
						      content: "내용",
						      thumbnailUrl: "https://example.com/image.png",
						    };
						    ```
							- `Post` 인터페이스에서 `"title"` 프로퍼티를 제외한 나머지 프로퍼티로 `noTitlePost` 타입을 정의합니다.
								- 직접 구현
								    ```typescript
								    type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
								    ```
									- `Exclude<keyof T, K>`
										- `T`의 키들에서 `K`를 제외한 키들을 추출
									- `Pick<T, ...>`
										- 추출된 키들로 새로운 타입을 만듦
					- Record 타입
						- `Record<K, V>`는 키 타입 `K`와 값 타입 `V`를 받아 해당 키들로 구성된 객체 타입을 생성하는 유틸리티 타입
						    ```typescript
						    type Thumbnail = Record<"large" | "medium" | "small", { url: string }>;
						
						    const thumbnails: Thumbnail = {
						      large: { url: "https://example.com/large.png" },
						      medium: { url: "https://example.com/medium.png" },
						      small: { url: "https://example.com/small.png" },
						    };
						    ```
							- `"large"`, `"medium"`, `"small"` 키를 가지고, 각 키의 값은 `{ url: string }`인 객체를 생성
								- 직접 구현
								    ```typescript
								    type Record<K extends keyof any, V> = {
								      [Key in K]: V;
								    };
								    ```
									- `K extends keyof any`
										- `K`는 모든 키 타입(string, number, symbol)을 확장해야함
									- `[Key in K]`
										- `K`에 있는 키들로 프로퍼티를 만듦
									- `V`
										- 모든 프로퍼티의 값 타입은 `V`
					- `Pick<T, K>`
						- 객체 타입 `T`에서 프로퍼티 `K`만 선택하여 새로운 타입을 생성
					- `Omit<T, K>`
						- 객체 타입 `T`에서 프로퍼티 `K`를 제외하고 새로운 타입을 생성
					- `Record<K, V>`
						- 키 타입 `K`와 값 타입 `V`로 구성된 객체 타입을 생성
			- 조건부 타입 기반의 유틸리티 타입 - Exclude, Extract, ReturnType
				- 조건부 타입 기반의 유틸리티 타입 - Exclude, Extract, ReturnType
					- Exclude 타입
						- `Exclude<T, U>`는 타입 `T`에서 타입 `U`를 제외하는 유틸리티 타입
							```typescript
							type A = Exclude<string | boolean, boolean>;
							// 결과: A는 string 타입
							```
						    - `string | boolean` 타입에서 `boolean`을 제외하면 `string` 타입만 남음
						  - 직접 구현
						    ```typescript
						    type Exclude<T, U> = T extends U ? never : T;
						    ```
						      - `T`가 `U`에 할당될 수 있으면 `never`를 반환
							      - 그렇지 않으면 `T`를 반환
					      - 유니온 타입에 분산적으로 적용되어 각 구성 요소에 대해 조건부 타입이 평가됨
					- Extract 타입
						- `Extract<T, U>`는 타입 `T`에서 타입 `U`에 할당될 수 있는 타입만 추출하는 유틸리티 타입
							```typescript
							type B = Extract<string | boolean, boolean>;
							// 결과: B는 boolean 타입
							```
						    - `string | boolean` 타입에서 `boolean` 타입만 추출
						- 직접 구현
							```typescript
							type Extract<T, U> = T extends U ? T : never;
							```
							- `T`가 `U`에 할당될 수 있으면 `T`를 반환
								- 그렇지 않으면 `never`를 반환
					- ReturnType 타입
						- `ReturnType<T>`는 함수 타입 `T`의 반환 타입을 추출하는 유틸리티 타입
						    ```typescript
						    function funcA() {
						      return "hello";
						    }
						
						    function funcB() {
						      return 42;
						    }
						
						    type ReturnA = ReturnType<typeof funcA>;
						    // 결과: ReturnA는 string 타입
						
						    type ReturnB = ReturnType<typeof funcB>;
						    // 결과: ReturnB는 number 타입
						    ```
						- 직접 구현
							```typescript
							type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
							```
							- `infer R`을 사용하여 함수의 반환 타입 `R`을 추론
							- 만약 `T`가 함수 타입이 아니면 `never`를 반환
					- `Exclude<T, U>`
						- 타입 `T`에서 `U`를 제외한 타입을 생성
					- `Extract<T, U>`
						- 타입 `T`에서 `U`에 할당될 수 있는 타입만 추출
					- `ReturnType<T>`
						- 함수 타입 `T`의 반환 타입을 추출
					- 이러한 유틸리티 타입들은 조건부 타입과 `infer` 키워드를 활용하여 직접 구현 가능
		
	- 섹션 12. 리액트와 타입스크립트
		- 섹션 12. 리액트와 타입스크립트
			- 타입스크립트 리액트 시작하기
				- 타입스크립트 리액트 시작하기
					- TypeScript 환경에서 React를 개발하는 방법 소개
						- TypeScript를 사용하는 React 프로젝트 설정
							- 터미널 → 다음 명령어를 실행하여 React 앱 생성
							  ```bash
							  npx create-react-app .
							  ```
							  - `.`은 현재 폴더에 React 앱을 생성
						- 불필요한 파일 삭제
							- `src` 디렉터리에서 다음 파일들 삭제
								- `App.test.js`
								- `logo.svg`
								- `reportWebVitals.js`
								- `setupTests.js`
								- `index.js`
									- 삭제된 파일과 관련된 `import` 구문과 코드를 제거
								- `App.js`
									- 삭제된 파일과 관련된 `import` 구문과 코드를 제거
						- 타입 선언 패키지 설치
							- 터미널에서 다음 명령어를 실행 → 타입 선언 패키지를 설치
							  ```bash
							  npm install @types/node @types/react @types/react-dom @types/jest
							  ```
							- 설치된 패키지는 `package.json`의 `devDependencies`에 추가
						- `tsconfig.json` 파일 생성 및 설정
							- 프로젝트 루트 디렉터리에 `tsconfig.json` 파일을 생성
							- 다음과 같이 컴파일러 옵션을 설정
							  ```json
							  {
							    "compilerOptions": {
							      "target": "ES5",
							      "module": "CommonJS",
							      "strict": true,
							      "allowJs": true,
							      "esModuleInterop": true,
							      "jsx": "react-jsx"
							    },
							    "include": ["src"]
							  }
				  ```
							  - `target`과 `module`을 설정하여 호환성을 높임
							  - `strict`를 `true`로 설정하여 엄격한 타입 검사를 활성화
							  - `allowJs`를 `true`로 설정하여 JavaScript 파일도 컴파일 대상에 포함
							  - `esModuleInterop`을 `true`로 설정하여 CommonJS 모듈 호환성을 제공
							  - `jsx`를 `react-jsx`로 설정하여 JSX 문법을 해석
						- 파일 확장자 변경
							- JSX 문법을 사용하는 파일의 확장자를 `.jsx`로 변경
								- `index.js` ⇒ `index.jsx`
								- `App.js` ⇒ `App.jsx`
						- TypeScript 파일로 변환 및 타입 오류 해결
							- 파일들을 하나씩 TypeScript 파일로 변환
								- `index.jsx` ⇒ `index.tsx`
								- `App.jsx` ⇒ `App.tsx`
							- 변환한 파일에서 발생하는 타입 오류를 해결
								- `esModuleInterop` 옵션 사용
									- 모듈 import 오류를 해결
								- `jsx` 옵션을 설정
									- JSX 문법 오류 해결
								- `document.getElementById`가 `null`을 반환할 수 있으므로
									- non-null 단언 연산자(`!`)나 타입 단언(`as HTMLElement`) 사용
					- JavaScript 프로젝트를 TypeScript로 마이그레이션할 때는 파일별로 하나씩 변환하고 오류를 해결하는 방식이 효과적
				
				
				
			- 상태관리와 Props 1
				- 상태관리와 Props 1
					- React에서 제공하는 기본적인 기능인 useState와 props를 TypeScript에서 사용하는 방법
					
					1. 상태 관리 (useState)
						useState를 사용하여 사용자가 입력한 텍스트를 상태로 관리
						```typescript
						const [text, setText] = useState('');
						```
						- 초기값을 설정하면 TypeScript가 자동으로 해당 상태의 타입을 추론
						- 초기값이 없을 경우 undefined로 추론
							- 타입을 명시해주는 것이 좋음
					2. 이벤트 핸들러 작성
						- 사용자가 입력한 값을 상태에 반영하기 위해 input 태그와 onChange 이벤트 핸들러를 추가
						```typescript
						  const onChangeInput = (e: React.ChangeEvent<HTMLInputElement>) => {
						    setText(e.target.value);
						  };
						```
					3. 할 일 추가 기능
						- todos 상태를 생성하여 할 일 목록을 관리
						```typescript
						const [todos, setTodos] = useState<Todo[]>([]);
						```
						  - Todo 객체의 구조를 정의
							  - 새로운 할 일을 추가할 때 id와 내용을 설정
						  - 고유한 id 값을 관리하기 위해 useRef를 사용
						```typescript
						const idRef = useRef(0);
						```
					4. 추가 버튼 이벤트
						- onClickAdd 함수
							- 사용자가 입력한 할 일을 추가
								- 추가 후 입력 필드를 초기화
						```typescript
						const onClickAdd = () => {
						setTodos([...todos, { id: idRef.current++, contents: text }]);
						setText('');
						};
						```
					5. 컴포넌트 분리 및 Props 사용
						- 입력 필드와 버튼을 Editor 컴포넌트로 분리
							- 재사용성을 높임
						- App.tsx에서 Editor 컴포넌트에 props로 onClickAdd 함수를 전달
						```typescript
						<Editor onClickAdd={onClickAdd} />
						```
						- props의 타입을 명시적으로 정의
							- 안전한 데이터 전달을 보장
						```typescript
						interface Props {
						onClickAdd: (text: string) => void;
						}
						```
					6. 결과 확인
						- useEffect로 todos 상태가 변경될 때마다 콘솔에 출력하여 상태 변화를 확인
				
			- 상태관리와 Props 2
				- 상태관리와 Props 2
					- 투두리스트 앱에 리스트 렌더링과 삭제 기능을 추가
					- useState에서 useReducer로 상태 관리를 업그레이드하는 방법
					
					1. 리스트 렌더링
						- todos 배열에 저장된 할 일들을 화면에 렌더링
						```tsx
						{todos.map(todo => (
						<TodoItem key={todo.id} {...todo} />
						))}
						```
						- TodoItem 컴포넌트로 할 일 아이템을 별도로 분리
							- props로 전달받은 값을 렌더링
					2. 공통 타입 분리
						- 여러 컴포넌트에서 사용하는 타입을 types.ts 파일로 분리하여 관리
							- Todo 타입을 여러 컴포넌트에서 재사용 가능하도록
						```typescript
						export interface Todo {
						id: number;
						contents: string;
						}
						  ```
					3. 삭제 기능
						- TodoItem 컴포넌트에서 삭제 버튼을 추가
							- 버튼이 클릭되면 App.tsx에서 전달한 onClickDelete 함수가 호출되도록 설정
						```tsx
						const onClickDelete = (id: number) => {
						setTodos(todos.filter(todo => todo.id !== id));
						};
						```
						- props로 onClickDelete 함수를 전달하여 삭제 기능을 구현
					4. useReducer로 상태 관리 변경
						- useState 대신 useReducer를 사용
							- 상태 관리 로직을 좀 더 구조화된 방식으로
						- reducer 함수
							- 상태와 액션을 받아서 상태를 업데이트하는 로직을 처리
						```typescript
						function reducer(state: Todo[], action: Action) {
						switch (action.type) {
						case 'CREATE':
						return [...state, action.data];
						case 'DELETE':
						return state.filter(todo => todo.id !== action.id);
						default:
						return state;
						}
						}
						```
						- dispatch를 사용해 액션을 전달
							- 상태를 변경
						```tsx
						dispatch({ type: 'CREATE', data: { id: idRef.current++, contents: text } });
					  ```
					5. 타입 안정성
						- useReducer를 사용할 때 Action 객체를 타입으로 명확하게 정의
							- 오류를 방지
						  ```typescript
						  type Action =
						    | { type: 'CREATE'; data: Todo }
						    | { type: 'DELETE'; id: number };
						  ```
				
			- Context API
				- Context API
					- React의 Context API를 TypeScript 환경에서 사용하는 방법
					- 기존 투두리스트 앱을 Context API를 활용하여 리팩토링
					
					- Context 생성
						- 두 개의 컨텍스트 생성
							- ToDoStateContext
								- 할 일 목록(`todos` 배열)을 관리
							- ToDoDispatchContext
								- 상태 변화 함수(`onClickAdd`, `onClickDelete`)를 관리
							```typescript
							export const ToDoStateContext = React.createContext<Todo[] | null>(null);
							export const ToDoDispatchContext = React.createContext<{
							onClickAdd: (text: string) => void;
							onClickDelete: (id: number) => void;
							} | null>(null);
							```
						- 타입 정의
							- 컨텍스트의 초기값을 `null`로 설정
							- TypeScript로 타입을 명확하게 지정
								- 이를 통해 `todos` 배열과 상태 변화 함수들이 전달될 수 있도록 설정
						- Provider 설정
							- `App.tsx`에서 `ToDoStateContext.Provider`와 `ToDoDispatchContext.Provider`를 사용
								- 상태와 함수를 컴포넌트 트리에 제공
							```tsx
							<ToDoStateContext.Provider value={todos}>
							<ToDoDispatchContext.Provider value={{ onClickAdd, onClickDelete }}>
							  {/* Components */}
							</ToDoDispatchContext.Provider>
							</ToDoStateContext.Provider>
							```
						- `useContext`로 값 사용
							- `useContext` 훅을 사용하여 필요한 곳에서 컨텍스트로부터 값을 가져옴
								- `Editor` 컴포넌트와 `ToDoItem` 컴포넌트에서 `dispatch` 함수를 사용
							```tsx
							const dispatch = useContext(ToDoDispatchContext);
							```
						- 커스텀 훅으로 코드 개선
							- `dispatch`가 `null`일 수 있으므로, 오류 처리 필요
							- 커스텀 훅 `useToDoDispatch`를 만들어 `null` 검사를 수행
								- 오류를 던지도록 구현
								```typescript
								function useToDoDispatch() {
								const dispatch = useContext(ToDoDispatchContext);
								if (!dispatch) {
								  throw new Error("ToDoDispatchContext가 제공되지 않았습니다.");
								}
								return dispatch;
								}
								```
						- `Editor`와 `ToDoItem` 컴포넌트
							- `props`로 전달받던 `onClickAdd`와 `onClickDelete` 함수를 제거
						- `useToDoDispatch` 훅을 사용
							- 상태 변화 함수를 호출하도록 수정
							  ```tsx
							  const { onClickAdd } = useToDoDispatch();
							  ```
			- 외부 라이브러리 사용하기
				- 외부 라이브러리 사용하기
					- TypeScript 프로젝트에서 외부 라이브러리를 설치하고 타입 오류 없이 사용하는 방법
					
					- TypeScript와 외부 라이브러리
					    - JavaScript만 사용하던 때는 `npm`을 통해 패키지를 설치하고 바로 사용
						    - TypeScript에서는 타입 검사가 필요
							    - 설치 후에 바로 사용할 수 없는 경우 존재
					- TypeScript를 지원하는 라이브러리
					    - TypeScript로 작성된 라이브러리들은 설치 후 바로 사용 가능
						    - `react-router-dom`은 TypeScript로 작성되어 있으므로 설치 후 바로 사용 가능
					  - 타입 정보가 없는 라이브러리
						- Lodash와 같은 JavaScript로 작성된 라이브러리는 타입 정보가 기본적으로 제공되지 않
							- 이런 라이브러리는 타입 선언 파일을 별도로 설치해야 함
								- `lodash`를 설치한 후 추가로 `@types/lodash`라는 타입 선언 패키지를 설치해야 TypeScript에서 오류 없이 사용 가능
					  - DefinitelyTyped
						- DefinitelyTyped는 JavaScript 라이브러리에 대한 타입 선언 파일을 제공하는 오픈 소스 저장소
							- `@types`로 시작하는 패키지들은 DefinitelyTyped에서 제공하는 타입 선언 파일
								- 이를 설치해 라이브러리의 타입을 해결 가능
									- `@types/node`, `@types/react`, `@types/react-dom` 등은 각각 Node.js, React의 타입 정보를 제공하는 패키지
					  - 설치 및 사용 방법
						- 먼저 라이브러리를 설치한 후, 해당 라이브러리의 타입 선언 패키지가 있는지 확인
							- 있다면 `npm i @types/라이브러리명` 명령어로 설치
						- 다음, TypeScript 파일에서 라이브러리를 임포트하여 사용 가능
					- TypeScript 프로젝트에서는 JavaScript 라이브러리를 바로 사용할 수 없으며, 추가로 타입 선언 패키지를 설치해야 할 수도 있음
					- TypeScript로 작성된 라이브러리는 설치 후 바로 사용 가능하지만, 그렇지 않은 경우 DefinitelyTyped에서 제공하는 `@types/라이브러리명` 패키지를 설치해야 
						- 이를 통해 TypeScript에서도 오류 없이 외부 라이브러리를 사용 가능
		
			- 타입스크립트 템플릿 소개
				- 타입스크립트 템플릿 소개
					- TypeScript로 React 프로젝트를 빠르게 시작할 수 있는 방법
						- Create React App(CRA)의 TypeScript 템플릿을 사용
					- Create React App (CRA)
						- CRA는 React 프로젝트를 빠르게 생성해주는 도구
						- `--template typescript` 옵션 사용
							- 처음부터 TypeScript가 적용된 React 프로젝트를 만들기 가능
					- 설치 방법
						- 명령어: `npx create-react-app . --template typescript`
							- TypeScript 템플릿을 사용하는 React 앱이 자동으로 생성
							- 생성된 프로젝트는 `.tsx` 파일을 사용
								- TypeScript 설정이 이미 구성
					- 자동 설정
						- tsconfig.json
							- 다양한 TypeScript 옵션들이 자동으로 설정
								- ES Module Interop 등
						- package.json
							- React, ReactDOM, Node, Jest와 같은 필수 패키지가 자동으로 설치
						- 기본 App.tsx 파일도 함께 제공

