# TypeScript

프엔코스 시작하면서 “이번기회에 타입스크립트도 제대로 배워봐라” 라는 소리를 들었다.
이번 강의는 기본 개념 익히는 강의라 코드를 쭉 같이 따라 쳐봤다.

실습환경 REPL을 쓰고 싶으면

```jsx
npx ts-node
```

## 타입 지정

```jsx
let name: string;
let age: number;

name = "주키";
age = 2;

let cat: {
  name: string,
  age: number,
};

cat = { name: "주키", age: 2 };
```

```jsx
type Cat: {
 name: string;
 age: number
};

let munchkin: Cat;
munchkin = { name: "주키", age: 2};

interface Kitty: {
 name: string;
 age: number
};

let siamese: Kitty;
siamese = { name: "브루키", age: 1};
```

```jsx
function add(x: number, y: number): number {
  return x + y;
}
```

```jsx
// 배열은 타입 뒤에 대괄호를 붙여준다.
let numbers: number[];
numbers = [1, 2, 3];

// 더 깐깐한 관리는 Tuple
let sound = [string, number];
sound = ["미야오", 1107];
```

## 타입추론

간단한 타입의 경우 자동으로 데이터 타입 추론해준다.

## Union Type

```jsx
type Bool = true | false;

let flag: Mool;

flag = true;

flag = false;
```

매개변수를 제한하거나 할 때 매우 유용하게 쓸 수 있다.

```tsx
type Category = "food" | "toy" | "bag";

function fetchProducts({ category }: { category: Category }) {
  console.log(`Fetch ${category}`);
}
```

레거시 환경 또는 코드 때문에 안 쓸 수가 없다. ReactNode 같은 게 대표적이다.

- [React Types](https://github.com/facebook/react/blob/main/packages/shared/ReactTypes.js)

```tsx
let target: string | number;

target = "홍길동";

target = 13;

target = null;

target = undefined;

let targetName: string | undefined;

targetName = "홍길동";

targetName = undefined;
```

그냥 undefined를 쓸 일은 없고, 함수 매개변수(parameters)에서 사용되곤 한다. 하지만 이보다는 물음표 기호(?)를 써서 [Optional Parameter](https://www.typescriptlang.org/docs/handbook/2/functions.html#optional-parameters)로 처리하는 걸 추천한다.

```tsx
function greeting(name?: string): string {
  return `Hello, ${name || "world"}`;
}
```

기본값을 잡아주면 더 좋다.

```tsx
function greeting(name: string = "world"): string {
  return `Hello, ${name}`;
}
```

매개변수가 오브젝트일 때도 활용할 수 있다.

```tsx
function greeting({ name, age }: { name: string; age?: number }): string {
  return age ? `${name} (${age})` : name;
}
```

→ 이렇게 쓰면 ts-node에선 아쉽게도 해석 불가하다.

아래처럼 한 줄로 붙여서 쓰거나, type 등으로 따로 잡아주면 된다.

```tsx
function greeting({ name, age }: { name: string; age?: number }): string {
  return age ? `${name} (${age})` : name;
}
```

```tsx
type Human = {
  name: string;
  age?: number;
};

function greeting({ name, age }: Human): string {
  return age ? `${name} (${age})` : name;
}

greeting();

greeting({ name: "홍길동" });

greeting({ name: "홍길동", age: 13 });
```

### Intersection Type

- [교집합](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes-func.html#%EA%B5%90%EC%A7%91%ED%95%A9)
- [Intersection Types](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types)

타입을 확장하는 간단한 방법.

```tsx
type Human = {
  name: string;
  age: number;
};

type Creature = {
  hp: number;
  mp: number;
};

type Person = Human & Creature;

let person: Person;

person = { name: "홍길동", age: 13, hp: 256, mp: 16 };
```
