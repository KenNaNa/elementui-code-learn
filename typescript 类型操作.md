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
