- 섹션 5. 함수와 타입
	- 함수 타입
		1. 기본 함수 타입 정의
		   - 매개변수와 반환값의 타입을 지정
		   ```typescript
		   function add(a: number, b: number): number {
		     return a + b;
		   }
		   ```
		2. 화살표 함수 타입 정의
		   - 기본 함수와 유사하게 정의
		   ```typescript
		   const multiply = (a: number, b: number): number => {
		     return a * b;
		   };
		   ```
		3. 매개변수 기본값
		   - 기본값을 설정하면 타입이 자동으로 추론
		   ```typescript
		   function greet(name = "이정환") {
		     console.log(`안녕하세요, ${name}님!`);
		   }
		   ```
		4. 선택적 매개변수
		   - 매개변수 이름 뒤에 '?'를 붙여 선택적으로 만듦
		   ```typescript
		   function introduce(name: string, tall?: number) {
		     console.log(`이름: ${name} ${tall ? ` 키는: ${tall}cm` : ''}`);
		   }
		   ```
		5. 나머지 매개변수(Rest Parameters)
		   - 여러 인수를 배열로 받음
		   ```typescript
		   function getSum(...numbers: number[]): number {
		     return numbers.reduce((sum, num) => sum + num, 0);
		   }
		   ```
		6. 튜플 타입을 이용한 나머지 매개변수
		   - 받을 인수의 개수와 타입을 정확히 지정 가능
		   ```typescript
		   function getSum(...numbers: [number, number, number]): number {
		     return numbers.reduce((sum, num) => sum + num, 0);
		   }
		   ```
		- 선택적 매개변수는 필수 매개변수 뒤에 위치해야 함
		- 타입 추론을 활용하면 코드를 간결하게 유지 가능
		- 매개변수의 타입과 기본값의 타입이 일치해야 함
	- 함수 타입 표현식과 호출 시그니쳐
		1. 함수 타입 표현식 (Function Type Expression)
		   - 함수의 타입을 별도로 정의하는 방법
		   - 타입 별칭을 사용하여 정의 가능
		     ```typescript
		     type Operation = (a: number, b: number) => number;
		     
		     const add: Operation = (a, b) => a + b;
		     const subtract: Operation = (a, b) => a - b;
		     ```
		   - 타입 별칭 없이도 직접 사용 가능
		     ```typescript
		     const multiply: (a: number, b: number) => number = (a, b) => a * b;
		     ```
		2. 호출 시그니처 (Call Signature)
		   - 함수의 타입을 객체 형태로 정의하는 방법
		     ```typescript
		     type Operation2 = {
		       (a: number, b: number): number;
		     };
		     
		     const addTo: Operation2 = (a, b) => a + b;
		     const subtractTo: Operation2 = (a, b) => a - b;
		     ```
		   - 함수가 JavaScript에서 객체이기 때문에 이런 형태로 정의 가능
		3. 하이브리드 타입
		   - 호출 시그니처를 사용하면서 추가 프로퍼티 정의 가능
		   - 함수이면서 동시에 객체로 사용 가능
		     ```typescript
		     type Operation2 = {
		       (a: number, b: number): number;
		       description: string;
		     };
		     
		     const addTo: Operation2 = (a, b) => a + b;
		     addTo.description = "덧셈 함수";
		     ```
		- 함수 타입 표현식과 호출 시그니처는 함수의 타입을 별도로 정의하는 방법
			- 사용 시 중복되는 타입 정의 줄이고 더 깔끔한 코드 작성 가능
		- 호출 시그니처는 함수가 JavaScript에서 객체라는 특성을 반영
		- 하이브리드 타입을 사용하면 함수와 객체의 특성을 모두 가진 타입 정의 가능
	- 함수 타입의 호환성
		1. 함수 타입 호환성의 두 가지 기준
			a) 반환값의 타입 호환성
			b) 매개변수의 타입 호환성
		2. 반환값의 타입 호환성
			- 업캐스팅(더 넓은 타입으로의 변환) : 허용
			- 다운캐스팅(더 좁은 타입으로의 변환) :  허용되지 않음
		3. 매개변수의 타입 호환성
			a) 매개변수의 개수가 같을 때
			- 업캐스팅 : 허용되지 않음
			- 다운캐스팅 : 허용
			b) 매개변수의 개수가 다를 때
			- 할당하려는 함수의 매개변수 개수가 더 적을 때만 허용
			- 매개변수의 타입은 일치해야 함
		4. 매개변수가 객체 타입일 때
			- 더 구체적인 타입(서브타입)의 함수를 더 일반적인 타입(슈퍼타입)의 함수에 할당할 수 없음
			- 반대로, 더 일반적인 타입의 함수를 더 구체적인 타입의 함수에 할당할 수 있음
		- 타입 안정성을 보장하기 위함
		- 예상치 못한 런타임 에러를 방지하기 위함
	- 함수 오버로딩
		1. 개념
		   - 동일한 이름의 함수를 매개변수의 개수나 타입에 따라 여러 버전으로 정의하는 방법
		   - JavaScript에서는 지원되지 않지만, TypeScript에서 사용 가능
		2. 구현 방법
		   a) 오버로드 시그니처 (Overload Signatures)
		      - 함수의 여러 버전을 선언하는 부분
		      - 구현부 없이 함수의 타입만 정의
		        ```typescript
		        function func(a: number): void;
		        function func(a: number, b: number, c: number): void;
		        ```
		   b) 구현 시그니처 (Implementation Signature)
			- 실제 함수의 구현 부분
			- 모든 오버로드 케이스를 처리할 수 있어야 함
		        ```typescript
		        function func(a: number, b?: number, c?: number): void {
		          if (typeof b === 'number' && typeof c === 'number') {
		            console.log(a + b + c);
		          } else {
		            console.log(a * 20);
		          }
		        }
		        ```
		   - 구현 시그니처의 매개변수는 모든 오버로드 시그니처와 호환되어야 함
		   - 선택적 매개변수를 사용하여 다양한 케이스 처리 가능
		   - 함수 호출 시 전달된 인수의 개수와 타입에 따라 적절한 오버로드 시그니처가 선택됨
		   - 구현 시그니처의 매개변수 정의는 호출 시 영향을 미치지 않음
		   - 다양한 상황에서 같은 함수를 유연하게 사용 가능
		   - 많은 TypeScript 라이브러리에서 사용되는 기법
	- 사용자 정의 타입 가드
		1. 개념
		   - 개발자가 직접 만든 함수를 이용해 타입을 좁히는 방법
		   - 복잡한 타입 검사를 함수로 분리하여 코드의 가독성과 재사용성을 높임
		2. 사용 상황
		   - 유니온 타입에서 특정 타입으로 좁히기 어려운 경우
		   - 기존 타입 가드 방식(instanceof, typeof 등)으로 충분하지 않을 때
		3. 구현 방법
		   ```typescript
		   function isDog(animal: Animal): animal is Dog {
		     return (animal as Dog).isBark !== undefined;
		   }
		   ```
		   - 함수의 반환 타입을 `parameterName is Type` 형태로 지정
		   - 함수가 true를 반환하면, 매개변수가 지정된 타입임을 보장
		4. 사용 예시
		   ```typescript
		   if (isDog(animal)) {
		     console.log(animal.isBark);  // animal이 Dog 타입으로 좁혀짐
		   } else {
		     console.log(animal.isScratch);  // animal이 Cat 타입으로 좁혀짐
		   }
		   ```
		   - 복잡한 타입 체크 로직을 함수로 분리하여 코드 가독성 향상
		   - 재사용 가능한 타입 가드 생성
		   - 타입스크립트가 타입을 정확히 추론할 수 있도록 도움
		   - 함수의 로직이 실제로 타입을 올바르게 체크하는지 확인 필요
		   - 과도한 사용은 코드 복잡성을 증가시킬 수 있음
- 섹션 6. 인터페이스
	- 인터페이스
		1. 개념
			- 객체 타입을 정의하는 데 특화된 문법
			- 상호 간의 약속된 규칙을 정의하는 역할
		2. 기본 문법
		   ```typescript
		   interface Person {
		     name: string;
		     age: number;
		   }
		   ```
		3. 주요 기능
		   - 선택적 프로퍼티: `age?: number;`
		   - 읽기 전용 프로퍼티: `readonly name: string;`
		   - 메서드 정의
		     ```typescript
		     sayHi: () => void;
		     // 또는
		     sayHi(): void;
		     ```
		4. 메서드 오버로딩
		   - 호출 시그니처를 사용해 구현
		   ```typescript
		   interface Person {
		     sayHi(): void;
		     sayHi(a: number, b: number): void;
		   }
		   ```
		5. 인터페이스 vs 타입 별칭
		   - 인터페이스는 유니온이나 인터섹션 타입을 직접 정의할 수 없음
		   - 객체 타입 정의에 더 특화됨
		6. 네이밍 컨벤션
		   - 일부에서는 'I'를 접두사로 사용 (예: IPerson)
		   - 팀이나 프로젝트의 컨벤션을 따르는 것이 중요
		7. 사용 예시
		   ```typescript
		   const person: Person = {
		     name: "이정환",
		     age: 27,
		     sayHi: () => console.log("Hi")
		   };
		   ```
		- 인터페이스는 타입스크립트에서 객체의 구조를 정의하는 강력한 도구
		- 타입 별칭과 유사, 하지만 객체 타입에 특화되어 있어 더 다양한 기능을 제공
			- 메서드 정의와 오버로딩에 유용
			- 코드의 가독성과 재사용성을 높이는 데 도움
	- 인터페이스 확장하기
		1. 개념
		   - 기존 인터페이스의 모든 프로퍼티를 포함하면서 새로운 프로퍼티를 추가하는 방법
		   - `extends` 키워드를 사용하여 구현
		2. 확장의 기본 문법
		   ```typescript
		   interface Animal {
			 name: string;
			 color: string;
		   }
		
		   interface Dog extends Animal {
			 isBark: boolean;
		   }
		   ```
		3. 장점
		   - 코드 중복 감소
		   - 타입 정의의 일관성 유지
		   - 유지보수성 향상
		4. 프로퍼티 재정의
		   - 확장된 인터페이스에서 기존 프로퍼티의 타입을 재정의할 수 있음
		   - 단, 재정의된 타입은 원본 타입의 서브타입이어야 함
		5. 다양한 확장 대상
		   - 인터페이스뿐만 아니라 타입 별칭으로 정의된 객체 타입도 확장 가능
		6. 다중 확장
		   - 여러 인터페이스를 동시에 확장 가능
		   ```typescript
		   interface DogCat extends Dog, Cat {
			 // 추가 프로퍼티
		   }
		   ```
		- 확장 시 프로퍼티 타입을 변경할 때는 원본 타입의 서브타입으로만 가능
		- 타입 호환성을 유지해야 함
	- 인터페이스 합치기
		1. 선언 합침의 개념
			- 동일한 이름의 인터페이스를 여러 번 선언 가능
			- 중복 선언된 인터페이스들은 자동으로 하나로 합쳐짐
		2. 타입 별칭과의 차이점
			- 타입 별칭은 중복 선언 불가능
			- 인터페이스는 중복 선언 가능
		3. 선언 합침의 예시
		   ```typescript
		   interface Person {
		     name: string;
		   }
		   interface Person {
		     age: number;
		   }
		   // 결과: Person 인터페이스는 name과 age 프로퍼티를 모두 가짐
		   ```
		4. 충돌 처리
			- 동일한 프로퍼티를 다른 타입으로 중복 정의하면 오류 발생
			- 중복 정의 시 반드시 동일한 타입으로 정의해야 함
		5. 인터페이스 확장과의 차이
			- 확장에서는 서브타입으로 재정의 가능
			- 선언 합침에서는 정확히 동일한 타입으로만 재정의 가능
		6. 주요 사용 사례
			- 모듈 보강(Declaration Merging)에 주로 사용
			- 라이브러리의 타입 정의를 확장하거나 수정할 때 유용
		7. 모듈 보강 예시
		   ```typescript
		   // 기존 라이브러리 정의
		   interface Lib {
		     a: number;
		     b: number;
		   }
		
		   // 사용자 정의 확장
		   interface Lib {
		     c: string;
		   }
		   // 결과: Lib 인터페이스는 a, b, c 프로퍼티를 모두 가짐
		   ```
		- 인터페이스의 선언 합침은 타입스크립트의 유연성을 보여주는 기능
			- 특히 외부 라이브러리의 타입을 확장하거나 수정할 때 유용
			- 하지만 일반적인 프로그래밍에서는 자주 사용되지 않으며, 주로 모듈 보강 작업에서 활용
- 섹션 7. 클래스
	- 자바스크립트의 클래스 소개
		1. 클래스의 개념
			- 객체를 생성하기 위한 틀 또는 설계도
			- 동일한 구조의 객체를 쉽게 여러 개 만들기 가능
		2. 클래스 정의
		   ```javascript
		   class Student {
		     // 클래스 내용
		   }
		   ```
		3. 클래스의 구성 요소
		   a) 필드: 객체의 속성(프로퍼티)을 정의
		   b) 생성자(Constructor): 객체 생성 시 초기화를 담당
		   c) 메서드: 객체의 기능을 정의
		4. 클래스 사용
		   ```javascript
		   const student = new Student("이정환", "A+", 27);
		   ```
		5. this 키워드
		   - 현재 객체를 가리킴
		   - 메서드 내에서 객체의 프로퍼티에 접근할 때 사용
		6. 클래스 상속
		   ```javascript
		   class StudentDeveloper extends Student {
		     // 추가적인 필드, 메서드 정의
		   }
		   ```
		   - `extends` 키워드로 다른 클래스를 상속
		   - 부모 클래스의 기능을 재사용하고 확장 가능
		7. super 키워드
		   - 부모 클래스의 생성자를 호출할 때 사용
		   - `super(name, grade, age);`
		8. 클래스 사용의 이점
		   - 코드 재사용성 증가
		   - 구조화된 코드 작성 가능
		   - 객체 지향 프로그래밍 구현 용이
	- 타입스크립트의 클래스
		1. 클래스 기본 구조
			- 필드: 클래스의 속성을 정의하며, 타입을 명시해야 함
			- 생성자: 객체 초기화를 담당, 필드 값을 설정
			- 메서드: 클래스의 기능을 정의
		2. 필드 선언
		   ```typescript
		   class Employee {
		     name: string;
		     age: number;
		     position: string;
		   }
		   ```
		3. 생성자
		   ```typescript
		   constructor(name: string, age: number, position: string) {
		     this.name = name;
		     this.age = age;
		     this.position = position;
		   }
		   ```
		4. 클래스의 이중 역할
		   - 자바스크립트 클래스로 사용 가능
		   - 타입으로도 사용 가능
		5. 클래스를 타입으로 사용
		   ```typescript
		   const employeeC: Employee = {
		     name: "홍길동",
		     age: 30,
		     position: "",
		     work() { /* ... */ }
		   };
		   ```
		6. 클래스 상속
		   ```typescript
		   class ExecutiveOfficer extends Employee {
		     officeNumber: string;
		
		     constructor(name: string, age: number, position: string, officeNumber: string) {
		       super(name, age, position);
		       this.officeNumber = officeNumber;
		     }
		   }
		   ```
		7. 타입스크립트의 특징
			- 필드에 타입을 명시해야 함
			- 생성자에서 모든 필드를 초기화해야 함
			- 상속 시 super() 호출이 필수
			- 타입 체크가 더 엄격하여 안전한 코드 작성 가능
		8. noImplicitAny 옵션
			- 암시적 any 타입 할당을 방지
			- 더 안전한 코드 작성을 위해 true로 설정 권장
	- 접근 제어자
		1. 접근 제어자(Access Modifiers)의 개념
			- 클래스의 필드나 메서드에 대한 접근 범위를 설정하는 기능
			- public, private, protected 세 가지 종류가 있음
		2.  public (기본값)
			- 어디서든 자유롭게 접근 가능
			- 명시적으로 작성하지 않아도 기본적으로 적용됨
			- `public name: string;`
		3. private
			- 해당 클래스 내부에서만 접근 가능
			- 클래스 외부나 파생 클래스에서 접근 불가
			- `private age: number;`
		4. protected
			- 해당 클래스와 파생 클래스 내부에서만 접근 가능
			- 클래스 외부에서는 접근 불가
			- `protected position: string;`
		5. 생성자 매개변수에 접근 제어자 사용
			- 자동으로 같은 이름의 필드를 생성하고 초기화함
			- `constructor(private name: string, protected age: number) {}`
			- 이 경우 별도로 필드를 선언하거나 this로 할당할 필요 없음
		6. 접근 제어자의 활용
			- 데이터 은닉화와 캡슐화를 위해 사용
			- 객체 지향 프로그래밍의 중요한 원칙을 구현하는 데 도움
			- 클래스 내부 구현을 외부로부터 보호하여 안정성 향상
		7. 주의사항
			- private 멤버는 파생 클래스에서도 접근 불가
			- protected 멤버는 파생 클래스에서 접근 가능하지만 외부에서는 불가
	- 인터페이스와 클래스
		1. 인터페이스를 이용한 클래스 설계
			- 인터페이스는 클래스의 구조를 정의하는 "설계도" 역할
			- 클래스가 특정 구조를 따르도록 강제 가능
		2. implements 키워드
			- 클래스가 특정 인터페이스를 구현함을 선언
			- `class Character implements CharacterInterface { ... }`
		3. 인터페이스 구현 시 주의사항
			- 인터페이스에 정의된 모든 속성과 메서드를 클래스에서 구현해야 함
			- 인터페이스의 모든 멤버는 기본적으로 public으로 간주
		4. 클래스에서의 구현
			- 인터페이스에 정의된 속성과 메서드를 클래스에서 정확히 구현해야 함
			- 생성자의 매개변수에 접근 제어자를 사용하면 자동으로 필드가 생성되고 초기화
		5. private 또는 protected 멤버
			- 인터페이스에서는 정의할 수 없으며, 필요한 경우 클래스 내부에서 별도로 정의해야 함
		6. 사용 예시
		   ```typescript
		   interface CharacterInterface {
		     name: string;
		     moveSpeed: number;
		     move(): void;
		   }
		
		   class Character implements CharacterInterface {
		     constructor(public name: string, public moveSpeed: number) {}
		
		     move() {
		       console.log(`${this.moveSpeed}만큼 속도로 이동`);
		     }
		   }
		   ```
			- 복잡한 시스템이나 라이브러리 설계 시 유용
			- 코드의 구조를 명확히 하고, 타입 안정성을 높임
- 섹션 8. 제네릭
- 섹션 9. 타입 조작하기
