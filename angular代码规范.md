@typescript-eslint/adjacent-overload-signatures#
重载的函数必须写在一起

增加可读性

【plugin-lint是否支持】：【是】

```js
declare namespace NSFoo2 {
    export function foo(s: string): void;
    export function foo(n: number): void;
    export function foo(sn: string | number): void;
    export function bar(): void;
}

type TypeFoo2 = {
    foo(s: string): void;
    foo(n: number): void;
    foo(sn: string | number): void;
    bar(): void;
};

interface IFoo2 {
    foo(s: string): void;
    foo(n: number): void;
    foo(sn: string | number): void;
    bar(): void;
}
```

@typescript-eslint/array-type#
限制数组类型必须使用 Array<T> 或 T[]

允许灵活运用两者

【plugin-lint是否支持】：【是】
  

@typescript-eslint/await-thenable#
禁止对没有 then 方法的对象使用 await

统一关闭 requires type information 的规则

【plugin-lint是否支持】：【是】
  
  
@typescript-eslint/ban-ts-ignore#
是否允许使用 // @ts-ignore 来忽略编译错误

既然已经使用注释来忽略编译错误了，说明已经清楚可能带来的后果

【plugin-lint是否支持】：【是】
  
@typescript-eslint/ban-types#
禁止使用指定的类型

【plugin-lint是否支持】：【是】
  
@typescript-eslint/camelcase#
变量名必须是 camelcase 风格的

很多 api 或文件名都不是 camelcase 风格的

【plugin-lint是否支持】：【是】
  
@typescript-eslint/class-name-casing#
类名与接口名必须为 PascalCase

【plugin-lint是否支持】：【是】
  
```ts
class ValidClassName {}

interface ValidInterface {}
```
  
@typescript-eslint/consistent-type-assertions#
类型断言必须使用 as Type，禁止使用 <Type>，禁止对对象字面量进行类型断言（断言成 any 是允许的）

<Type> 容易被理解为 jsx

配置：
["error", {
    "assertionStyle": "as",
    "objectLiteralTypeAssertions": "never"
}]
【plugin-lint是否支持】：【是】
  
```ts
let bar2: string | number;
const foo2 = bar2 as string;

const baz2 = {
    bar: 1
} as any;
```
