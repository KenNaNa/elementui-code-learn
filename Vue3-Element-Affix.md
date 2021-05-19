Vue3 类型检测

基础props验证看Vue3官网props介绍即可：点击传送 ，官网没有对特定类型属性进行补充。

# PropType 定义

```js
export declare type Prop<T, D = T> = PropOptions<T, D> | PropType<T>;

declare type PropConstructor<T = any> = {
    new (...args: any[]): T & object;
} | {
    (): T;
} | PropMethod<T>;

declare type PropMethod<T, TConstructor = any> = T extends (...args: any) => any ? {
    new (): TConstructor;
    (): T;
    readonly prototype: TConstructor;
} : never;

declare interface PropOptions<T = any, D = T> {
    type?: PropType<T> | true | null;
    required?: boolean;
    default?: D | DefaultFactory<D> | null | undefined | object;
    validator?(value: unknown): boolean;
}

export declare type PropType<T> = PropConstructor<T> | PropConstructor<T>[];
```

# PropType 使用

假如我有一个todoItem 组件，需要item 信息属性，需要准守 TodoItem interface。

1、 引入

```js
import { PropType } from 'vue'
```

2、 定义接口

```js
export interface TodoItem {
  text: string
  done: boolean
}
```
3、 属性验证

```js
 props: {
    todo: {
      type: Object as PropType<TodoItem>,
      default: {
        text: '',
        done: false
      }
    }
  },
```






























