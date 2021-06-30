## Keyof 类型运算符

```ts
type Point = { x: number; y: number };
type P = keyof Point;

const x: P = 'x';
const y: P = 'y';
```

如果类型具有string或number索引签名，keyof则将返回这些类型：

```ts
type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish;

const a: A = 1;


type Mapish = { [k: string]: boolean };
type M = keyof Mapish;
const m: M = '111';
const k: M = 111;
```

请注意，在此示例中，Mis string | number— 这是因为 JavaScript 对象键始终被强制转换为字符串，因此obj[0]始终与obj["0"].

## Typeof 类型运算符

typeof类型的操作

JavaScript 已经有一个typeof可以在表达式上下文中使用的运算符：

```ts
console.log(typeof 'Hello world');

type Predicate = (x: unknown) => boolean;
type K = ReturnType<Predicate>;
```

值与类型不一样

```ts
function f() {
  return { x: 10, y: 3 };
}
type P = ReturnType<typeof f>;

const p: P = { x:10, y: 3 }
```

## 索引访问类型

```ts
type Person = {
  age: number;
  name: string;
  alive: boolean;
};
type Age = Person['age'];

const age: Age = 10;


type Person = {
  age: number;
  name: string;
  alive: boolean;
};
type I1 = Person['age' | 'name'] // number | string
type I2 = Person[keyof Person] // number | string | boolean
type AliveOrName = "alive" | "name"
type I3 = Person[AliveOrName] // string | boolean
```

## 映射类型








