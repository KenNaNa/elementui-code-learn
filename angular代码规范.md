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
    
@typescript-eslint/consistent-type-definitions#
优先使用 interface 而不是 type

interface 可以 implement, extend 和 merge

配置：["error", "interface"]
【plugin-lint是否支持】：【是】
    
```ts
interface Foo2 {
    foo: string;
} 
```

@typescript-eslint/explicit-function-return-type#
函数返回值必须与声明的类型一致

编译阶段检查就足够了

【plugin-lint是否支持】：【是】

```ts
    
```
@typescript-eslint/explicit-member-accessibility#
必须设置类的成员的可访问性

将不需要公开的成员设为私有的，可以增强代码的可理解性，对文档输出也很友好

【plugin-lint是否支持】：【是】
  
    
```ts
class Foo2 {
    private static foo = 'foo';
    public static getFoo() {
        return Foo2.foo;
    }
    public constructor() {}
    protected bar = 'bar';
    public getBar() {}
    public get baz() {
        return 'baz';
    }
    public set baz(value) {
        console.log(value);
    }
} 
```
@typescript-eslint/generic-type-naming#
约束泛型的命名规则

【plugin-lint是否支持】：【是】

@typescript-eslint/interface-name-prefix#
接口名称必须以 I 开头

【plugin-lint是否支持】：【是】

@typescript-eslint/member-naming#
私有成员必须以 _ 开头

已有 private 修饰符了，没必要限制变量名

【plugin-lint是否支持】：【是】

@typescript-eslint/member-ordering#
指定类成员的排序规则

优先级：<br/>1. static > instance<br/>2. field > constructor > method<br/>3. public > protected > private

配置：
["error", {
    "default": [
        "public-static-field",
        "protected-static-field",
        "private-static-field",
        "static-field",
        "public-static-method",
        "protected-static-method",
        "private-static-method",
        "static-method",
        "public-instance-field",
        "protected-instance-field",
        "private-instance-field",
        "public-field",
        "protected-field",
        "private-field",
        "instance-field",
        "field",
        "constructor",
        "public-instance-method",
        "protected-instance-method",
        "private-instance-method",
        "public-method",
        "protected-method",
        "private-method",
        "instance-method",
        "method"
    ]
}]
【plugin-lint是否支持】：【是】
```ts
class Foo2 {
    public static foo1 = 'foo1';
    protected static foo2 = 'foo2';
    private static foo3 = 'foo3';
    public static getFoo1() {}
    protected static getFoo2() {}
    private static getFoo3() {
        return Foo2.foo3;
    }
    public bar1 = 'bar1';
    protected bar2 = 'bar2';
    private bar3 = 'bar3';
    public constructor() {
        console.log(Foo2.getFoo3());
        console.log(this.getBar3());
    }
    public getBar1() {}
    protected getBar2() {}
    private getBar3() {
        return this.bar3;
    }
} 
```
@typescript-eslint/no-array-constructor#
禁止使用 Array 构造函数

【plugin-lint是否支持】：【是】

@typescript-eslint/no-dynamic-delete#
禁止 delete 时传入的 key 是动态的

【plugin-lint是否支持】：【是】

@typescript-eslint/no-empty-function#
不允许有空函数

有时需要将一个空函数设置为某个项的默认值

【plugin-lint是否支持】：【是】
 
@typescript-eslint/no-empty-interface#
禁止定义空的接口

【plugin-lint是否支持】：【是】
```ts
interface Foo2 {
    foo: string;
} 
```
@typescript-eslint/no-explicit-any#
禁止使用 any

【plugin-lint是否支持】：【是】

@typescript-eslint/no-extra-non-null-assertion#
禁止多余的 non-null 断言

【plugin-lint是否支持】：【是】

@typescript-eslint/no-extraneous-class#
禁止定义没必要的类，比如只有静态方法的类

【plugin-lint是否支持】：【是】

@typescript-eslint/no-floating-promises#
禁止调用 Promise 时没有处理异常情况

【plugin-lint是否支持】：【是】

@typescript-eslint/no-for-in-array#
禁止对 array 使用 for in 循环

统一关闭 requires type information 的规则

【plugin-lint是否支持】：【是】
