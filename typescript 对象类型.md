在 JavaScript 中，我们分组和传递数据的基本方式是通过对象。在 TypeScript 中，我们通过对象类型来表示那些。

正如我们所见，它们可以是匿名的：

```ts
function greet(person: { name: string; age: number }) {
  return 'Hello ' + person.name;
}
```

我们可以将上面的函数的参数抽取出来

```ts
interface Person {
  name: string;
  age: number;
}

type Person {
  name: string;
  age: number;
}
```

可选属性

```ts
// 可选属性
interface PaintOptions {
  xPos?: number;
  yPos?: number;
}

function paintShape(opts: PaintOptions) {
  let xPos = opts.xPos;

  let yPos = opts.yPos;
}
```

只读属性

```ts
// 只读类型
interface Person {
  readonly age: number;
}
```

索引签名

```ts
// 索引签名
interface StringArray {
  [index: number]: string;
}
const myArray: StringArray = ['1', '2', '4'];
const secondItem = myArray[1];
```

扩展类型

拥有可能是其他类型的更具体版本的类型是很常见的。例如，我们可能有一个BasicAddress类型来描述在美国发送信件和包裹所需的字段

```ts
interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}
```
在某些情况下，这就足够了，但如果地址处的建筑物有多个单元，则地址通常具有与其关联的单元号。然后我们可以描述一个AddressWithUnit.

```ts
interface AddressWithUnit {
  name?: string;
  unit: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}
```

这可以完成工作，但这里的缺点是我们必须重复所有其他字段，BasicAddress因为我们的更改纯粹是可添加的。相反，我们可以扩展原始BasicAddress类型，只需添加AddressWithUnit.

```ts
interface BasicAddress {
  name?: string;
  street: string;
  city: string;
  country: string;
  postalCode: string;
}

interface AddressWithUnit extends BasicAddress {
  unit: string;
}
```
