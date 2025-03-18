## 기본 타입
* string, number, boolean: 기본적인 문자열, 숫자, 불리언 타입을 나타냄
* null, undefined: 값이 없거나 정의되지 않은 값을 나타냄
* array, tuple: 배열은 동일한 타입의 원소를 여러 개 포함하는 것이고, 튜플은 각각 다른 타입의 원소를 순서대로 포함함
* enum: 명명된 상수의 집합을 정의할 때 사용됨
* void: 아무 값도 반환하지 않는 함수의 반환 타입으로 사용됨
* never: 절대 발생하지 않는 값의 타입을 나타냄
* object: 비 원시 타입을 나타내며 객체, 배열, 함수, 클래스의 인스턴스 등을 포함함
* any: 모든 타입을 허용하는 타입

```javascript
let str: string = "Hello TypeScript";
let num: number = 42;
let bool: boolean = true;
let arr: number[] = [1, 2, 3];
let tuple: [string, number] = ["John", 25];
enum Colors { Red, Green, Blue }

function logMessage(message: string): void {
  console.log(message);
}
function throwError(message: string): never {
  throw new Error(message);
}
function infiniteLoop(): never {
  while (true) {
    // 무한 루프
  }
}

let c: Colors = Colors.Red;
let notSure: any = 4;
notSure = "maybe a string";
```

## 타입 연산자 및 고급 타입
* Literal Types: 정확한 값의 타입을 나타냄
* Union: 두 개 이상의 타입 중 하나를 선택할 수 있는 타입을 나타냄
* Type Narrowing: 특정 스코프 내에서 변수의 타입을 좁히는 것입. 종종 타입 가드와 함께 사용됨
* Intersection: 두 개 이상의 타입을 모두 만족하는 타입을 정의함
* Type Alias: 새로운 타입의 이름을 정의함
* Type Assertion: 이미 있는 변수의 타입을 강제로 변경함
* typeof, keyof: 변수의 타입 또는 객체의 키의 타입을 가져옴
* Mapped types: 기존 타입을 변환하여 새로운 타입을 만듬
* Conditional types: 조건에 따라 타입을 결정함

```javascript
//Literal Types
let literalVar: "A" | "B" = "A";
let age: 20 | 30 | 40 = 20;

//Union
type UnionType = string | number;
let unionVar: UnionType = "hello";
unionVar = 3;

//Type Narrowing
function example(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toUpperCase()); // value는 이 스코프에서 string
  } else {
    console.log(value.toFixed(2)); // value는 이 스코프에서 number
  }
}

//Intersection
type IntersectionType = { id: number } & { name: string };
let intersectionVar: IntersectionType = { id: 1, name: "John" };

//Type Alias
type AliasType = string | null;
let aliasVar: AliasType = "hello";

//Type Assertion
let someValue: any = "this is a string";
let strLength: number = (someValue as string).length;

// typeof
let var1: number = 100;
type VarType = typeof var1; // 'number'

// keyof
type ObjType = { a: number; b: string; };
type KeyType = keyof ObjType; // 'a' | 'b'

// Mapped types
type OriginalType = { a: number; b: string; };
type MappedType = {
  [P in keyof OriginalType]: boolean;
}; // 결과: { a: boolean; b: boolean; }

// Conditional types
type IsString<T> = T extends string ? true : false;
type Var1 = IsString<"hello">; // true
type Var2 = IsString<number>; // false
```

## 속성 선언
* readonly: 객체의 속성을 수정 불가능하게 만듬
* optional: 객체의 속성이 선택적으로 포함될 수 있음을 나타냄

```javascript
interface Config {
    readonly path: string;
    port?: number;
}
```

## 함수와 메서드
* Optional, Default, Rest Parameters: 선택적 매개변수, 기본 매개변수 값, 여러 매개변수를 배열로 처리하는 방법을 나타냄
* 함수 오버로딩: 같은 이름의 함수를 다른 매개변수와 반환 타입으로 여러 번 정의하는 것을 나타냄

```javascript
  //Optional
  function optional(name?: string): void {
    if (name) {
      console.log(`Hello, ${name}!`);
    } else {
      console.log("Hello!");
    }[
  }
  optional(); // 출력: Hello!
  optional("Alice"); // 출력: Hello, Alice!

  //Default
  function defaultFun(name: string = "Raphael"): void {
    if (name) {
      console.log(`Hello, ${name}!`);
    } else {
    console.log("Hello!");
    }
  }

  defaultFun(); // 출력: Hello, Raphael!
  defaultFun("Alice"); // 출력: Hello, Alice!

  //Rest Parameters
  function rest(...numbers: number[]): void {
    for (let num of numbers) {](url)
      console.log(num);
    }
  }
  rest(1, 2, 3, 4, 5); // 순서대로 1, 2, 3, 4, 5 출력


 //함수 오버로딩
  function overload(value: string): string;
  function overload(value: number): number;
  function overload(value: any): any {
    if (typeof value === "string") {
      return value + " string";
    } else {
      return value + 10;
    }
  }
```

## 인터페이스와 클래스
* interface, class: 데이터와 그 데이터를 작업하는 메서드의 형태를 정의함 (형태만 정의하냐 구현체를 생성하냐 차이)
* implements: 클래스가 특정 인터페이스를 구현하겠다고 명시할 때 사용
* extends: 한 클래스가 다른 클래스의 속성과 메소드를 상속받을 때 사용, 또한 인터페이스가 다른 인터페이스를 확장할 때도 사용
* 접근 제한자: 클래스의 속성과 메서드의 접근 수준을 지정함
* abstract: 추상 클래스와 메서드를 정의함(추상 클래스와 그 멤버를 정의, 추상 클래스는 직접 인스턴스화될 수 없고. 상속을 받은 클래스에서 오버라이딩해줌)
* static: 클래스 수준에서 작동하는 속성과 메서드를 정의함(클래스의 인스턴스를 생성하지 않고도 사용할 수 있는 클래스 수준의 속성 및 메서드를 정의)

```javascript
{
  interface Animal {
    name: string;
    makeSound(): void;
  }

  class Dog implements Animal {
    name: string;
    constructor(name: string) {
      this.name = name;
    }
    makeSound() {
      console.log("Woof!");](url)
    }
  }

  class Poodle extends Dog {
    style: string;
    constructor(name: string, style: string) {
      super(name);
      this.style = style;
    }
  }
}

{
  //접근 제한자
  class Example {
    public pubVar: string = "public variable";
    private privVar: string = "private variable";
    protected protVar: string = "protected variable";
  }

  //static
  class StaticExample {
    static staticVar: string = "Static Variable";

    static staticMethod(): void {
      console.log("This is a static method.");
    }
  }

  //추상 클래스
  abstract class Animal {
    abstract makeSound(): void; // 추상 메서드

    move(): void {
      console.log("Moving...");
    }
  }

  // 파생 클래스 (Dog)
  class Dog extends Animal {
    // 추상 메서드 makeSound() 오버라이딩
    makeSound(): void {
      console.log("Woof! Woof!");
    }
  }

  // 파생 클래스 (Cat)
  class Cat extends Animal {
    // 추상 메서드 makeSound() 오버라이딩
    makeSound(): void {
      console.log("Meow! Meow!");
    }
  }

  const dog: Animal = new Dog();
  dog.makeSound(); // 출력: Woof! Woof!

  const cat: Animal = new Cat();
  cat.makeSound(); // 출력: Meow! Meow!
}

```

## 제네릭
* 제네릭 함수, 인터페이스, 클래스: 타입을 매개변수로 취하는 코드를 작성함
* 제네릭 제약: 제네릭 타입에 조건을 부여함

```javascript
function identity<T>(value: T): T {
    return value;
}

let numberIdentity: number = identity<number>(123);
let stringIdentity: string = identity<string>("Hello");

console.log(numberIdentity); // 출력: 123
console.log(stringIdentity); // 출력: Hello
```

```javascript
// 제네릭 제약 예시
interface HasLength {
    length: number;
}

function identityWithConstraint<T extends HasLength>(value: T): T {
    console.log(value.length); // length 속성에 접근할 수 있습니다.
    return value;
}

let arrayIdentity = identityWithConstraint([1, 2, 3]); // 배열은 length 속성을 가지고 있음
console.log(arrayIdentity); // 출력: [1, 2, 3]

let objectIdentity = identityWithConstraint({length: 10, width: 20}); // 객체에도 length 속성이 있음
console.log(objectIdentity); // 출력: { length: 10, width: 20 }
```

## 인덱스 시그니처 (Index Signatures)
* 객체의 모든 속성에 대한 타입을 한 번에 정의
* 동적 속성을 가진 객체를 타입 안전하게 정의할 때 유용
* 키와 값의 타입을 지정할 수 있음

```javascript

// 기본 인덱스 시그니처
interface StringMap {
    [key: string]: string;
}

const names: StringMap = {
    "100": "John",
    "200": "Jane",
    "300": "Bob"
};

// 복합 인덱스 시그니처
interface NumberDictionary {
    [index: string]: number;
    length: number;    // OK
    // name: string;   // Error: 인덱스 시그니처가 number이므로 string 불가
}

// 여러 타입을 허용하는 인덱스 시그니처
interface MixedDictionary {
    [key: string]: string | number;
    id: number;        // OK
    name: string;      // OK
}
```

## 타입 단언 (Type Assertions)
* 컴파일러에게 개발자가 타입을 더 잘 알고 있음을 알리는 방법
* 'as' 키워드나 angle-bracket(<>) 문법 사용
* 타입 캐스팅과는 다름 (런타임에 영향을 주지 않음)

```javascript
// as 문법
let someValue: unknown = "Hello World";
let strLength: number = (someValue as string).length;

// angle-bracket 문법 (JSX에서는 사용 불가)
let someValue: unknown = "Hello World";
let strLength: number = (<string>someValue).length;

// 복합 타입 단언
interface Person {
    name: string;
    age: number;
}

const body = document.body;
const person = body as unknown as Person;

// const 단언
const point = { x: 3, y: 4 } as const;
// point는 { readonly x: 3, readonly y: 4 } 타입이 됨
```

### as A / as unknown as A 차이 
* 가능하면 타입 단언을 피하고 타입 가드를 사용
* 타입 단언이 필요한 경우


```javascript
function isCustomElement(element: Element): element is CustomElement {
  return 'customProperty' in element && 'customMethod' in element;
}

const element = document.getElementById('myElement');
if (element && isCustomElement(element)) {
  element.customMethod(); // 타입 안전!
}
```

```javascript
// 예시 1: API 응답 데이터 변환
interface UserData {
  id: number;
  name: string;
}

// 🚨 직접 변환 시 에러 발생 가능
const userData1 = JSON.parse(response) as UserData;

// ✅ unknown을 통한 안전한 변환
const userData2 = JSON.parse(response) as unknown as UserData;

// 예시 2: DOM 요소 타입 변환
interface CustomElement {
  customProperty: string;
  customMethod(): void;
}

// 🚨 직접 변환 시 에러 발생
const element1 = document.getElementById('myElement') as CustomElement;

// ✅ unknown을 통한 안전한 변환
const element2 = document.getElementById('myElement') as unknown as CustomElement;
```

#### as A
* 직접적인 타입 단언
* 더 위험할 수 있음
* TypeScript는 body와 Person 사이에 겹치는 속성이 있어야 함
* 두 타입 간에 전혀 관계가 없다면 컴파일 에러가 발생할 수 있음

```javascript
interface Person {
  name: string;
  age: number;
}

// 이 경우 에러가 발생할 수 있음
const person = document.body as Person; // 🚨 Type 'HTMLBodyElement' is missing properties...
```

#### as unknown as A
* 이중 단언(double assertion)
* 더 안전함
* unknown을 중간 단계로 사용하여 모든 타입으로의 변환을 허용
* TypeScript의 타입 검사를 우회할 수 있음

```javascript
interface Person {
  name: string;
  age: number;
}

// 컴파일 에러 없이 동작
const person = document.body as unknown as Person; // ✅ OK
```


## 타입 추론 (Type Inference)
* TypeScript가 자동으로 타입을 추론하는 방식을 이해하는 것이 중요
* 변수 초기화, 함수 반환 값, 제네릭 등에서 발생
* 명시적 타입 선언이 없어도 컨텍스트에 따라 타입이 결정됨

```javascript
// 변수 타입 추론
let message = "hello"; // string으로 추론
let numbers = [1, 2, 3]; // number[]로 추론

// 객체 타입 추론
let person = {
    name: "John",
    age: 30
}; // { name: string; age: number }로 추론

// 함수 반환 타입 추론
function add(x: number, y: number) {
    return x + y; // 반환 타입이 number로 추론
}

// 제네릭 타입 추론
let items = <T>(items: T[]) => items[0];
let result = items([1, 2, 3]); // number로 추론
```


## 타입 가드와 타입 추론
* 사용자 정의 타입 가드: 특정 타입이 맞는지 확인하는 함수를 정의함
* 타입 캐스팅과 타입 확정: 변수의 타입을 명시적으로 지정하거나 확인함

```javascript
interface Fish {
    swim: () => void;
    layEggs: () => void;
}

interface Bird {
    fly: () => void;
    layEggs: () => void;
}

// 사용자 정의 타입 가드
function isFish(pet: Fish | Bird): pet is Fish {
    return (pet as Fish).swim !== undefined;
}

// 타입 가드를 활용하여 특정 동작을 수행
function move(pet: Fish | Bird): void {
    if (isFish(pet)) {
        pet.swim();
    } else {
        pet.fly();
    }
}

const myFish: Fish = {
    swim: () => console.log('swimming...'),
    layEggs: () => console.log('laying eggs...')
};

const myBird: Bird = {
    fly: () => console.log('flying...'),
    layEggs: () => console.log('laying eggs...')
};

move(myFish); // 출력: swimming...
move(myBird); // 출력: flying...

```

## 데코레이터
* 클래스, 메서드, 속성, 매개변수 데코레이터: 메타데이터를 추가하거나 클래스 및 그 멤버의 동작을 수정함
* 데코레이터 팩토리: 데코레이터를 생성하는 함수

```javascript
function log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    // ... log decorator logic
}
class MyClass {
    @log
    method() { /*...*/ }
}

```

## 환경 설정과 컴파일러 옵션
* tsconfig.json: TypeScript 프로젝트의 설정 파일
* 주요 컴파일러 옵션: 프로젝트의 컴파일 방식을 지정하는 옵션

```javascript
{
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "strict": true,
        "outDir": "./dist"
    },
    "include": ["src/**/*.ts"],
    "exclude": ["node_modules"]
}
```

## 템플릿 리터럴 타입
* 문자열 리터럴 타입을 기반으로 새로운 문자열 타입을 생성
* 타입 레벨에서 문자열 조작이 가능
* 유니온 타입과 함께 사용하여 강력한 타입 제한 가능

```javascript
// 기본 템플릿 리터럴 타입
type Greeting = `Hello ${string}`;
let greeting: Greeting = "Hello World"; // OK
let invalid: Greeting = "Hi World";     // Error

// 유니온 타입과 함께 사용
type Direction = "up" | "down";
type Position = "top" | "bottom";
type DirectionPosition = `${Direction}-${Position}`;
// "up-top" | "up-bottom" | "down-top" | "down-bottom"

// 실제 사용 예시
type EventName = "click" | "focus" | "blur";
type EventHandler = `on${Capitalize<EventName>}`;
// "onClick" | "onFocus" | "onBlur"

// CSS 속성 타입 예시
type CSSValue = number | string;
type CSSProperty = `${string}-${string}`;
type CSSStyles = Record<CSSProperty, CSSValue>;

const styles: CSSStyles = {
    "font-size": 16,
    "background-color": "#fff"
};
```
## 유틸리티 타입
* Partial, Readonly 등: 기존 타입을 기반으로 변형된 타입을 만듬

```javascript
type Todo = {
    id: number;
    title: string;
    description: string;
    completed: boolean;
}

// Partial: 모든 속성을 선택적으로 만들기
type PartialTodo = Partial<Todo>;
  
// Readonly: 모든 속성을 readonly로 만들기
type ReadonlyTodo = Readonly<Todo>;

// Pick: 특정 속성만 선택하여 새로운 타입을 생성
type TodoPreview = Pick<Todo, "title" | "description">;

// Omit: 특정 속성을 제외하고 새로운 타입을 생성
type TodoWithoutDescription = Omit<Todo, "description">;

// 사용 예제
const updateTodo = (todo: Todo, fieldsToUpdate: PartialTodo): Todo => {
    return { ...todo, ...fieldsToUpdate };
}

const todo: Todo = {
    id: 1,
    title: "Learn TypeScript",
    description: "Study hard!",
    completed: false
};

const updatedTodo = updateTodo(todo, {
    description: "Study harder!"
});

console.log(updatedTodo); // 출력: { id: 1, title: 'Learn TypeScript', description: 'Study harder!', completed: false }
```

## Partial<T>
* 타입의 모든 속성을 선택적(optional)으로 만드는 유틸리티 타입
* 기존 타입의 모든 속성을 '?'를 붙여 선택적으로 만듦
* 부분적인 객체 타입을 만들 때 유용
* 객체의 일부 속성만 업데이트할 때 자주 사용됨

```javascript
// Partial 사용
type PartialUser = Partial<User>;
// 결과:
// {
//   id?: number;
//   name?: string;
//   age?: number;
//   email?: string;
// }

// 실제 사용 예시
function updateUser(user: User, updates: Partial<User>): User {
  return { ...user, ...updates };
}

const user: User = {
  id: 1,
  name: "John",
  age: 30,
  email: "john@example.com"
};

// 일부 속성만 업데이트
const updatedUser = updateUser(user, {
  age: 31,
  email: "john.doe@example.com"
});
```

## Record<K, T>
* 키 타입 K와 값 타입 T를 사용하여 객체 타입을 생성하는 유틸리티 타입
* 객체의 키와 값의 타입을 정의할 때 사용
* 일관된 타입의 객체를 만들 때 유용
* 특히 동적 키를 가진 객체를 타입 안전하게 정의할 때 자주 사용됨

```javascript
// 기본 사용 예시
type UserRoles = Record<string, boolean>;
const roles: UserRoles = {
  admin: true,
  editor: false,
  viewer: true
};

// enum과 함께 사용
enum UserStatus {
  Active = "ACTIVE",
  Inactive = "INACTIVE",
  Pending = "PENDING"
}

type UserStatusMessages = Record<UserStatus, string>;

const statusMessages: UserStatusMessages = {
  [UserStatus.Active]: "User is active",
  [UserStatus.Inactive]: "User is inactive",
  [UserStatus.Pending]: "User is pending activation"
};

// 복잡한 값 타입과 함께 사용
interface UserInfo {
  name: string;
  lastLogin: Date;
}

type UsersDatabase = Record<number, UserInfo>;

const users: UsersDatabase = {
  1: { name: "John", lastLogin: new Date() },
  2: { name: "Jane", lastLogin: new Date() }
};
```

## Required<T>
* 모든 속성을 필수로 만드는 유틸리티 타입
* 선택적 속성을 포함한 타입을 필수 속성으로 변환할 때 유용

```javascript
interface User {
  id?: number;
  name?: string;
  age?: number;
  email?: string;
}

// Required 사용
type RequiredUser = Required<User>;
// 결과:
// {
//   id: number;
//   name: string;
//   age: number;
//   email: string;
// }

// 실제 사용 예시
const user: RequiredUser = {
  id: 1,
  name: "John",
  age: 30,
  email: "john@example.com"
};
```

## Pick<T, K>
* 특정 속성만 선택하여 새로운 타입을 생성하는 유틸리티 타입
* 필요한 속성만 포함하는 타입을 만들 때 유용

```javascript
interface User {
  id: number;
  name: string;
  age: number;
  email: string;
}

// Pick 사용
type PickedUser = Pick<User, "id" | "name">;
// 결과:
// {
//   id: number;
//   name: string;
// }

// 실제 사용 예시
const user: PickedUser = {
  id: 1,
  name: "John"
};
```

## Exclude<T, U>
* 특정 속성만 선택하여 새로운 타입을 생성하는 유틸리티 타입
* 필요한 속성만 포함하는 타입을 만들 때 유용

```javascript
type AllTypes = string | number | boolean;

// Exclude 사용
type StringOrNumber = Exclude<AllTypes, boolean>;
// 결과: string | number

// 실제 사용 예시
let value: StringOrNumber = "Hello";
value = 42;
// value = true; // 오류 발생
```

## Extract<T, U>
* 타입 T에서 타입 U와 겹치는 부분만 추출하는 유틸리티 타입
* 두 타입 간의 교집합을 만들 때 유용

```javascript
type AllTypes = string | number | boolean;

// Extract 사용
type StringOrBoolean = Extract<AllTypes, string | boolean>;
// 결과: string | boolean

// 실제 사용 예시
let value: StringOrBoolean = "Hello";
value = true;
// value = 42; // 오류 발생
```

## NonNullable<T>
* null과 undefined를 제거하는 유틸리티 타입
* 항상 값이 있는 타입을 만들 때 유용

```javascript
type NullableString = string | null | undefined;

// NonNullable 사용
type NonNullableString = NonNullable<NullableString>;
// 결과: string

// 실제 사용 예시
let value: NonNullableString = "Hello";
// value = null; // 오류 발생
// value = undefined; // 오류 발생
```
