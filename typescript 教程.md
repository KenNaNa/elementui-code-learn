为什么需要类型检测呢？这玩意对于前端来说，我们是无感的，因为 js 体现出来就是没有类型，但是如果没有了类型检测，会发生什么奇怪的事情呢？

小明定义了一个变量赋值为数字，都是后面由于小明生病了，工作暂时交给小张处理，小张不小心将这个变量赋值改为字符串，导致程序报错，一时间找不到问题，头都大了。

尤其是大项目，团队多人合作开发的项目，更是如此重要

# 基本类型

### 字符串，数字，布尔

```ts
let name: string = '小仙女';
let age: number = 18;
let sex: boolean = false;
```

### 数组

```ts
let names: Array<string> = ['小仙女', 'Ken'];
let ages: number[] = [18, 20];
let sexs: boolean[] = [false, true];
let anys: any[] = ['1', 2, true];
```

### 函数参数类型，返回值类型

```ts
function call(me: string|number|boolean): any {
  return me;
}
```

### 对象类型

```ts
let obj: { x: number, y: number } = {x: 10, y: 100};
```

### 可选属性

```ts
// 基本类型
let name: string = '小仙女';
let age: number = 18;
let sex: boolean = false;

// 数组
let names: Array<string> = ['小仙女', 'Ken'];
let ages: number[] = [18, 20];
let sexs: boolean[] = [false, true];

// 函数
function call(me: string | number | boolean): any {
  return me;
}
// 对象类型
let obj: { x: number; y: number } = { x: 10, y: 100 };

// 可选属性
let person: { name: string; age: number; sex?: boolean } = { name, age };
```





