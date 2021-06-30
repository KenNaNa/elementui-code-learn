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
