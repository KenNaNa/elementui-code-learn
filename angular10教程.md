[Angular 修仙之路目录](https://github.com/semlinker/angular2-ionic2/blob/master/ANGULAR.md)

[angular-movie-app](https://github.com/marekdano/angular-movie-app)

1. 基础内容，像组件注册，事件，等
2. 如何编写组件
3. 生命周期

- 1、ngOnChanges - 当数据绑定输入属性的值发生变化时调用
- 2、ngOnInit - 在第一次 ngOnChanges 后调用
- 3、ngDoCheck - 自定义的方法，用于检测和处理值的改变
- 4、ngAfterContentInit - 在组件内容初始化之后调用
- 5、ngAfterContentChecked - 组件每次检查内容时调用
- 6、ngAfterViewInit - 组件相应的视图初始化之后调用
- 7、ngAfterViewChecked - 组件每次检查视图时调用
- 8、ngOnDestroy - 指令销毁前调用

# 父组件应用子组件的方法

在父组件使用 @childView 

```js
export class OnChangesParentComponent {
  hero!: Hero;
  power = '';
  title = 'OnChanges';
  @ViewChild(OnChangesComponent) childView!: OnChangesComponent;

  constructor() {
    this.reset();
  }

  reset() {
    // new Hero object every time; triggers onChanges
    this.hero = new Hero('Windstorm');
    // setting power only triggers onChanges if this value is different
    this.power = 'sing';
    if (this.childView) {
      this.childView.reset();
    }
  }
}
```

# typescript


