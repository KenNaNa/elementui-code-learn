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








