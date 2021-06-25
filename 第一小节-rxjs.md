# RxJS入门和概览

RxJS 是Reactive Extensions For JavaScript的简写，它是一个强大的 JavaScript Reactive 编程库。Reactive 是指响应式编程（Reactive Programming）。

# 什么是响应式编程（Reactive Programming）？

任何异步事件（比如页面鼠标 click 事件），在响应式编程都是异步事件流。不仅仅是 click、hover 这种事件，任何变量、用户输入、属性、缓存、数据结构等，响应式编程把所有事物都看成是数据流。数据流是类似数组一样的序列，可以像数组一样，用 merge、map、concat 等方法操作。简单来说就是：把所有事物都事件流化，然后把这些事件流像数组一样去操作，就是响应式编程。

RxJS 提供了各种 API 来创建数据流：

- 单值：of, empty, never
- 多值：from
- 定时：interval, timer
- 从事件创建：fromEvent
- 从 Promise 创建：fromPromise
- 自定义创建：create

创建出来的数据流，是一种类似数组一样的序列，可以被订阅，也可以用如下 API 操作控制：

- 改变数据形态：map, mapTo, pluck
- 过滤一些值：filter, skip, first, last, take
- 时间轴上的操作：delay, timeout, throttle, debounce, audit, bufferTime
- 累加：reduce, scan
- 异常处理：throw, catch, retry, finally
- 条件执行：takeUntil, delayWhen, retryWhen, subscribeOn, ObserveOn
- 转接：switch
- concat，保持原来的序列顺序连接两个数据流
- merge，合并序列
- race，预设条件为其中一个数据流完成
- forkJoin，预设条件为所有数据流都完成
- zip，取各来源数据流最后一个值合并为对象
- combineLatest，取各来源数据流最后一个值合并为数组

# 异步事件流

在上图中有如下几种东西：

- click 事件流
- 事件流产生的值
- 错误
- 事件流结束
- 时间轴


点击一个按钮事件，随着时间推移，这个点击事件会产生三个不同的结果：值，发生错误，事件完成。我们可以定义方法用来：捕获值，捕获错误，捕获点击事件结束。在这个过程中，涉及到以下几个 RxJS 的基本概念：

Observable(可观察对象)：就是点击事件流。

Observers(观察者)：就是捕获值/错误/事件结束的方法（其实就是回调函数集合)。

Subscription(订阅)：Observable 产生的值都需要通过一个‘监听’把值传给 Observers，这个‘监听’就是 Subscription。

生产者 (Producer)：就是点击事件，是事件的生产者。

我们也可以用 ASCII 来描述这个事件流：

--a---b-c---d---X---|->

a b c d 是产生的值
X 是错误
| 是事件结束标志
---> 是时间线

```js
import { Component, OnInit } from "@angular/core";
import { fromEvent, Subscription } from "rxjs";
import { map, scan, buffer, throttleTime, filter } from "rxjs/operators";

@Component({
  templateUrl: "./rxjs-learning.component.html",
})
export class RxjsLearningComponent implements OnInit {
  subscription: Subscription;

  ngOnInit() {
    this.getClickCounter();
    this.getDouleClickCounter();
  }

  getClickCounter() {
    let button = document.getElementById("myBtn");

    let clickStream$ = fromEvent(button, "click");

    let counterStream$ = clickStream$.pipe(
      map((data) => {
        console.log("click====>事件对象", data);
        return 1;
      }),
      scan((acc, curr) => acc + curr, 0)
    );

    counterStream$.subscribe((data) => {
      console.log("this is the click counter: " + data);
    });
  }

  getDouleClickCounter() {
    let button = document.getElementById("myBtn");

    let clickStream$ = fromEvent(button, "click");

    let doubleClickStream$ = clickStream$.pipe(
      buffer(clickStream$.pipe(throttleTime(250))),
      map((click) => {
        console.log("click====>", click);
        return click.length;
      }),
      filter((num) => num >= 2)
    );

    doubleClickStream$.subscribe((data) => {
      console.log("the number of double click is: " + data);
    });
  }
}
```











