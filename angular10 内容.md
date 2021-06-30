## encapsulation详解

```ts
Emulated： 默认值,样式只对当前组件生效
Native(ShadowDom)：使用原生的Shadow Dom
None: 全局
ViewEncapsulation.Emulated

代码：

@Component({
 selector: 'app-home',
 template: '<p class="home-test">home works!</p>',
 styles:[`
 .home-test{
   color:red;
 }
`],
 encapsulation: ViewEncapsulation.Emulated,
})
```

## onChanges, ngOnChanges, SimpleChange

当数据绑定输入属性的值发生变化的时候，Angular 将会主动调用 ngOnChanges 方法。它会获得一个 SimpleChanges 对象，包含绑定属性的新值和旧值，它主要用于监测组件输入属性的变化。具体示例如下：

这个例子是父组件引用一个子组件，子组件的页面上有计算器，根据父组件传递的初始值进行运行，进行加减1的运算。父组件可以多次传值修改子组件的初始值。

```ts
import { Component, Input, SimpleChanges, OnChanges } from '@angular/core';

@Component({
    selector: 'exe-Child',
    template: `
      <p>当前值: {{ count }}</p>
      <button (click)="increment()"> + </button>
      <button (click)="decrement()"> - </button>
    `
})
export class ChildComponent implements OnChanges{
    @Input() count: number = 0;


    ngOnChanges(changes: SimpleChanges) {
        console.dir(changes['count']);
    }

    increment() {
        this.count++;
    }

    decrement() {
        this.count--;
    }
}
```
## forwardRef，ElementRef

- [Angular2进阶(一)——如何避免Dom误区](https://blog.csdn.net/changyinling520/article/details/78014052)
- [Angular 正确使用 @ViewChild、@ViewChildren 访问 DOM、组件、指令](https://blog.csdn.net/wuyuxing24/article/details/84927282)








