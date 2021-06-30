## Partial<Type>

构造一个所有属性都Type设置为可选的类型。此实用程序将返回表示给定类型的所有子集的类型。

```ts
interface Todo {
  title: string;
  desc: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
  title: 'organize desk',
  desc: 'clear clutter',
};

const todo2 = updateTodo(todo1, {
  desc: 'throw out trash',
});
```
  
## Required<Type>

构造一个由Typeset to required的所有属性组成的类型
  
```ts

interface Props {
  a?: number;
  b?: string;
}

const obj: Props = { a: 5 };

const obj2: Required<Props> = { a: 5 };
```
## Readonly<Type>
构造一个所有属性都Type设置为的类型readonly，这意味着不能重新分配构造类型的属性。
```ts
interface Todo {
  title: string;
}

const todo: Readonly<Todo> = {
  title: "Delete inactive users",
};

todo.title = "Hello";
```
## Record<Keys,Type>
构造一个对象类型，其属性键为Keys，属性值为Type。此实用程序可用于将一种类型的属性映射到另一种类型。
  
```ts
interface CatInfo {
  age: number;
  breed: string;
}

type CatName = 'miffy' | 'boris' | 'mordred';

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: 'Persian' },
  boris: { age: 5, breed: 'Maine Coon' },
  mordred: { age: 16, breed: 'British Shorthair' },
};
```
## Pick<Type, Keys>
通过从 中选取一组属性Keys（字符串文字或字符串文字的并集）来构造一个类型Type。
```ts
interface Todo {
  title: string;
  desc: string;
  completed: boolean;
}
type TodoPreview = Pick<Todo, 'title' | 'desc'>;
const todo: TodoPreview = {
  title: 'clean room',
  desc: 'ddddd',
};
```
