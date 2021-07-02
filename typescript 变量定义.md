let和const是 JavaScript 中变量声明的两个相对较新的概念。 正如我们之前提到的，在某些方面let类似于var，但允许用户避免用户在 JavaScript 中遇到的一些常见“陷阱”。

const是letin的增强，因为它可以防止重新分配给变量。

由于 TypeScript 是 JavaScript 的扩展，因此该语言自然支持let和const. 在这里，我们将详细说明这些新声明以及为什么它们比var.

如果您随意使用 JavaScript，下一部分可能是刷新您记忆的好方法。如果您非常熟悉varJavaScript中声明的所有怪癖，您可能会发现跳过它会更容易。

### var 声明

在 JavaScript 中声明变量传统上一直使用var关键字来完成。

```ts
var a = 10;
```

您可能已经发现，我们只是声明了一个以avalue命名的变量10。

我们还可以在函数内部声明一个变量：

```ts
function f() {
  var message = "Hello, world!";
  return message;
}
```

我们还可以在其他函数中访问这些相同的变量：

```ts
function f() {
  var a = 10;
  return function g() {
    var b = a + 1;
    return b;
  };
}
var g = f();
g(); // returns '11'
```

在上面的这个例子中，g捕获了a在f. 在任何g被调用的点上， 的值都a将与ain的值相关联f。即使g被调用一次f运行完成，它也将能够访问和修改a.

```ts
function f() {
  var a = 1;
  a = 2;
  var b = g();
  a = 3;
  return b;
  function g() {
    return a;
  }
}
f(); // returns '2'
```

### 范围规则

var对于那些习惯于其他语言的声明，声明有一些奇怪的范围规则。以下面的例子为例：

```ts
function f(shouldInitialize: boolean) {
  if (shouldInitialize) {
    var x = 10;
  }
  return x;
}
f(true); // returns '10'
f(false); // returns 'undefined'
```

一些读者可能会对这个例子有双重看法。该变量x是在if块内声明的，但我们能够从该块外部访问它。这是因为var声明可以在其包含的函数、模块、命名空间或全局范围内的任何地方访问 - 我们稍后将讨论所有这些 - 无论包含块如何。有些人称之为var-scoping或function-scoping。参数也是函数范围的。

这些范围规则可能会导致多种类型的错误。它们加剧的一个问题是，多次声明同一个变量并不是错误：

```ts
function sumMatrix(matrix: number[][]) {
  var sum = 0;
  for (var i = 0; i < matrix.length; i++) {
    var currentRow = matrix[i];
    for (var i = 0; i < currentRow.length; i++) {
      sum += currentRow[i];
    }
  }
  return sum;
}
```


也许对于一些有经验的 JavaScript 开发人员来说很容易发现，但是内部for循环会意外地覆盖该变量，i因为它i引用了相同的函数作用域变量。正如经验丰富的开发人员现在所知道的那样，类似的错误会在代码审查中溜走，并且可能是无尽的挫败感。

### 变量捕获怪癖
花点时间猜测以下代码段的输出是什么：

```ts
for (var i = 0; i < 10; i++) {
  setTimeout(function () {
    console.log(i);
  }, 100 * i);
}
```

对于那些不熟悉的人，setTimeout会在一定的毫秒数后尝试执行一个函数（尽管等待其他任何东西停止运行）。

准备好？看一看：

```ts
10
10
10
10
10
10
10
10
10
10
```

还记得我们之前提到的关于变量捕获的内容吗？我们传递给的每个函数表达式setTimeout实际上都引用了i来自同一作用域的相同函数表达式。

让我们花点时间考虑一下这意味着什么。 setTimeout将在几毫秒后运行一个函数，但只有在for循环停止执行之后；由当时的for循环已经停止执行，价值i就是10。所以每次给定的函数被调用时，它都会打印出来10！

一个常见的解决方法是使用 IIFE - 一个立即调用的函数表达式 -i在每次迭代时捕获：

```ts
for (var i = 0; i < 10; i++) {
  // capture the current state of 'i'
  // by invoking a function with its current value
  (function (i) {
    setTimeout(function () {
      console.log(i);
    }, 100 * i);
  })(i);
}
```
这种看起来很奇怪的模式实际上很常见。该i参数列表实际的阴影i中宣告for循环，但由于我们将它们命名为相同的，我们没有太多修改循环体。

### let 声明

到目前为止，您已经发现var存在一些问题，这正是let引入语句的原因。除了使用的关键字外，let语句的编写方式与var语句相同。

```ts
let hello = "Hello!";
```

### 块作用域

当一个变量被声明为 using 时let，它使用一些所谓的词法作用域或块作用域。与声明的变量var的范围泄漏到其包含函数不同，块范围的变量在其最近的包含块或for-loop之外不可见。

```ts
function f(input: boolean) {
  let a = 100;
  if (input) {
    // Still okay to reference 'a'
    let b = a + 1;
    return b;
  }
  // Error: 'b' doesn't exist here
  return b;
}
```

在这里，我们有两个局部变量a和b。 a的范围仅限于fwhile的主体，其b范围仅限于包含if语句的块。

catch子句中声明的变量也有类似的作用域规则。

```ts
try {
  throw "oh no!";
} catch (e) {
  console.log("Oh well.");
}
// Error: 'e' doesn't exist here
console.log(e);
```
块作用域变量的另一个属性是它们在实际声明之前不能被读取或写入。虽然这些变量在其整个范围内“存在”，但直到它们声明为止的所有点都是它们的时间死区的一部分。这只是一种复杂的说法，你不能在let语句之前访问它们，幸运的是 TypeScript 会让你知道这一点。

```ts
a++; // illegal to use 'a' before it's declared;
let a;
```
需要注意的是，您仍然可以在声明之前捕获块范围的变量。唯一的问题是在声明之前调用该函数是非法的。如果针对 ES2015，现代运行时会抛出错误；但是，现在 TypeScript 是宽容的，不会将此报告为错误。

```ts
function foo() {
  // okay to capture 'a'
  return a;
}
// illegal call 'foo' before 'a' is declared
// runtimes should throw an error here
foo();
let a;
```
有关时间死区的更多信息，请参阅Mozilla 开发者网络上的相关内容。

### 重新声明和阴影
对于var声明，我们提到过声明变量的次数并不重要；你刚拿到一个。

```ts
function f(x) {
  var x;
  var x;
  if (true) {
    var x;
  }
}
```

在上面的例子中，所有的声明x实际上都指向同一个 x，这是完全有效的。这通常最终成为错误的来源。值得庆幸的是，let声明并没有那么宽容。

```ts
let x = 10;
let x = 20; // error: can't re-declare 'x' in the same scope
```

对于 TypeScript 来说，变量不一定都需要是块作用域才能告诉我们存在问题。

```ts
function f(x) {
  let x = 100; // error: interferes with parameter declaration
}
function g() {
  let x = 100;
  var x = 100; // error: can't have both declarations of 'x'
}
```

这并不是说永远不能用函数作用域变量声明块作用域变量。块范围变量只需要在明显不同的块中声明。

```ts
function f(condition, x) {
  if (condition) {
    let x = 100;
    return x;
  }
  return x;
}
f(false, 0); // returns '0'
f(true, 0); // returns '100'
```

在更多嵌套的作用域中引入新名称的行为称为shadowing。它有点像一把双刃剑，因为它可以在意外阴影的情况下自行引入某些错误，同时还可以防止某些错误。例如，假设我们已经sumMatrix使用let变量编写了我们之前的函数。

```ts
function sumMatrix(matrix: number[][]) {
  let sum = 0;
  for (let i = 0; i < matrix.length; i++) {
    var currentRow = matrix[i];
    for (let i = 0; i < currentRow.length; i++) {
      sum += currentRow[i];
    }
  }
  return sum;
}
```
这个版本的循环实际上会正确地执行求和，因为内循环的i阴影i来自外循环。

为了编写更清晰的代码，通常应该避免阴影。虽然在某些情况下可能适合利用它，但您应该使用最佳判断。

### 块范围变量捕获
当我们第一次接触到通过var声明捕获变量的想法时，我们简要地介绍了变量在被捕获后的行为。为了更好地理解这一点，每次运行范围时，它都会创建一个变量“环境”。即使在其范围内的所有内容都已完成执行之后，该环境及其捕获的变量仍然可以存在。

```ts
function theCityThatAlwaysSleeps() {
  let getCity;
  if (true) {
    let city = "Seattle";
    getCity = function () {
      return city;
    };
  }
  return getCity();
}
```

因为我们是city从它的环境中捕获的，所以尽管if块已完成执行，我们仍然可以访问它。

回想一下我们之前的setTimeout示例，我们最终需要使用 IIFE 来捕获for循环每次迭代的变量状态。实际上，我们所做的是为我们捕获的变量创建一个新的变量环境。这有点痛苦，但幸运的是，您再也不必在 TypeScript 中这样做了。

let当声明为循环的一部分时，声明具有截然不同的行为。这些声明不仅仅是为循环本身引入一个新环境，而是在每次迭代中创建一个新的作用域。由于这就是我们在 IIFE 中所做的事情，我们可以将旧setTimeout示例更改为仅使用let声明。

```ts
for (let i = 0; i < 10; i++) {
  setTimeout(function () {
    console.log(i);
  }, 100 * i);
}
```

正如预期的那样，这将打印出来

```ts
0
1
2
3
4
5
6
7
8
9
```
### const 声明
const 声明是另一种声明变量的方式。

```ts
const numLivesForCat = 9;
```
它们就像let声明，但正如它们的名字所暗示的那样，一旦它们被绑定，它们的值就不能改变。换句话说，它们具有与 相同的范围规则let，但您不能重新分配给它们。

这不应与它们所引用的值不可变的想法混淆。

```ts
const numLivesForCat = 9;
const kitty = {
  name: "Aurora",
  numLives: numLivesForCat,
};
// Error
kitty = {
  name: "Danielle",
  numLives: numLivesForCat,
};
// all "okay"
kitty.name = "Rory";
kitty.name = "Kitty";
kitty.name = "Cat";
kitty.numLives--;
```

### 解构
TypeScript 的另一个 ECMAScript 2015 特性是解构。有关完整参考，请参阅Mozilla 开发人员网络上的文章。在本节中，我们将简要概述。

数组解构
最简单的解构形式是数组解构赋值：

```ts
let input = [1, 2];
let [first, second] = input;
console.log(first); // outputs 1
console.log(second); // outputs 2
```


这将创建两个名为first和 的新变量second。这相当于使用索引，但更方便：
```ts
first = input[0];
second = input[1];
```
解构也适用于已经声明的变量：
```ts
// swap variables
[first, second] = [second, first];
```
并带有函数的参数：
```
function f([first, second]: [number, number]) {
  console.log(first);
  console.log(second);
}
f([1, 2]);
```
您可以使用以下语法为列表中的其余项目创建一个变量...：
```ts
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // outputs 1
console.log(rest); // outputs [ 2, 3, 4 ]
```
当然，由于这是 JavaScript，您可以忽略您不关心的尾随元素：
```
let [first] = [1, 2, 3, 4];
console.log(first); // outputs 1
```
或其他元素：
```ts
let [, second, , fourth] = [1, 2, 3, 4];
console.log(second); // outputs 2
console.log(fourth); // outputs 4
```

### 元组解构
元组可以像数组一样被解构；解构变量获取相应元组元素的类型：
```
let tuple: [number, string, boolean] = [7, "hello", true];
let [a, b, c] = tuple; // a: number, b: string, c: boolean
```
对超出其元素范围的元组进行解构是错误的：
```
let [a, b, c, d] = tuple; // Error, no element at index 3
```
与数组一样，您可以使用..., 对元组的其余部分进行解构，以获得更短的元组：
```
let [a, ...bc] = tuple; // bc: [string, boolean]
let [a, b, c, ...d] = tuple; // d: [], the empty tuple
```
或忽略尾随元素或其他元素：
```ts
let [a] = tuple; // a: number
let [, b] = tuple; // b: string
```

### 对象解构
您还可以解构对象：
```ts
let o = {
  a: "foo",
  b: 12,
  c: "bar",
};
let { a, b } = o;
```
这将创建新变量a和bfromo.a和o.b。请注意，c如果您不需要它，您可以跳过它。

与数组解构一样，您可以在没有声明的情况下进行赋值：
```ts
({ a, b } = { a: "baz", b: 101 });
```
请注意，我们必须用括号将这个语句括起来。JavaScript 通常将 a 解析{为块的开始。

您可以使用以下语法为对象中的其余项目创建变量...：
```ts
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
```
属性重命名
您还可以为属性指定不同的名称：
```ts
let { a: newName1, b: newName2 } = o;
```
这里的语法开始变得混乱。你可以读a: newName1作“a作为newName1”。方向是从左到右，就像你写的一样：
```ts
let newName1 = o.a;
let newName2 = o.b;
```
令人困惑的是，这里的冒号也没有注明型号。类型，如果指定了，在整个解构之后还需要写：
```ts
let { a, b }: { a: string; b: number } = o;
```
### 默认值
默认值允许您在未定义属性的情况下指定默认值：

function keepWholeObject(wholeObject: { a: string; b?: number }) {
  let { a, b = 1001 } = wholeObject;
}
在此示例中，b?指示b是可选的，因此它可能是undefined。 keepWholeObject现在有一个变量 forwholeObject以及属性aand b，即使b未定义。

### 函数声明
解构也适用于函数声明。对于简单的情况，这很简单：
```ts
type C = { a: string; b?: number };
function f({ a, b }: C): void {
  // ...
}
```
但是为参数指定默认值更常见，并且通过解构获得正确的默认值可能很棘手。首先，您需要记住将模式放在默认值之前。
```ts
function f({ a = "", b = 0 } = {}): void {
  // ...
}
f();
```
上面的代码片段是类型推断的一个例子，手册前面已经解释过。

然后，您需要记住在解构属性而不是主初始化程序上为可选属性提供默认值。请记住，它C是用b可选定义的：
```ts
function f({ a, b = 0 } = { a: "" }): void {
  // ...
}
f({ a: "yes" }); // ok, default b = 0
f(); // ok, default to { a: "" }, which then defaults b = 0
f({}); // error, 'a' is required if you supply an argument
```
小心使用解构。正如前面的例子所展示的，除了最简单的解构表达式之外，任何东西都是令人困惑的。这是深层嵌套的解构，它得到更是如此真的很难理解，即使没有打桩重命名，默认值和类型标注。尽量保持解构表达式小而简单。你总是可以自己编写解构会生成的作业。

### 传播
展开运算符与解构相反。它允许您将一个数组扩展到另一个数组中，或者将一个对象扩展到另一个对象中。例如：
```ts
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];
```
这给 bothPlus 值[0, 1, 2, 3, 4, 5]。传播创造了一个浅拷贝first和second。它们不会因价差而改变。

您还可以传播对象：
```ts
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { ...defaults, food: "rich" };
```
现在search是{ food: "rich", price: "$$", ambiance: "noisy" }。对象传播比数组传播更复杂。和数组展开一样，它是从左到右进行的，但结果仍然是一个对象。这意味着在扩展对象中较晚出现的属性会覆盖较早出现的属性。因此，如果我们将前面的示例修改为最后传播：
```ts
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { food: "rich", ...defaults };
```
然后overwrites 中的food属性，在这种情况下这不是我们想要的。defaultsfood: "rich"

对象传播还有一些其他令人惊讶的限制。首先，它只包含对象 自己的可枚举属性。基本上，这意味着在传播对象实例时会丢失方法：
```ts
class C {
  p = 12;
  m() {}
}
let c = new C();
let clone = { ...c };
clone.p; // ok
clone.m(); // error!
```
其次，TypeScript 编译器不允许从泛型函数传播类型参数。该功能预计会出现在该语言的未来版本中。



