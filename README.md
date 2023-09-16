# 타입스크립트 문법 정리

## 기본 타입
* string, number, boolean: 기본적인 문자열, 숫자, 불리언 타입을 나타냄
* null, undefined: 값이 없거나 정의되지 않은 값을 나타냄
* array, tuple: 배열은 동일한 타입의 원소를 여러 개 포함하는 것이고, 튜플은 각각 다른 타입의 원소를 순서대로 포함함
* enum: 명명된 상수의 집합을 정의할 때 사용됨
* void, never: void는 아무 값도 반환하지 않는 함수의 반환 타입으로 사용되며, never는 절대 발생하지 않는 값의 타입을 나타냄
* object: 비 원시 타입을 나타내며 객체, 배열, 함수, 클래스의 인스턴스 등을 포함함
* any: 모든 타입을 허용하는 타입

```javascript
let str: string = "Hello TypeScript";
let num: number = 42;
let bool: boolean = true;
let arr: number[] = [1, 2, 3];
let tuple: [string, number] = ["John", 25];
enum Colors { Red, Green, Blue }
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

### 참고
* https://www.typescriptlang.org/docs/handbook/intro.html
