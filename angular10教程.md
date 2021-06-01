[Angular 修仙之路目录](https://github.com/semlinker/angular2-ionic2/blob/master/ANGULAR.md)

[angular-movie-app](https://github.com/marekdano/angular-movie-app)

[what-is-a-service-in-angular-js-why-to-use-it](https://dzone.com/articles/what-is-a-service-in-angular-js-why-to-use-it#:~:text=Angular%20Services&text=The%20main%20objective%20of%20a,controller%20to%20another%20custom%20service)


Angular服务
Angular服务是单例对象，在应用程序的生存期内只实例化一次。它们包含在应用程序的整个生命周期中维护数据的方法，即数据不会刷新，并且一直可用。服务的主要目标是与Angular应用程序的不同组件组织和共享业务逻辑、模型或数据和函数。

何时使用服务的一个示例是将数据从一个控制器传输到另一个自定义服务。

为什么我们应该在Angular中使用服务？
关注点的分离是Angular服务出现的主要原因。Angular服务是一个无状态对象，提供了一些非常有用的功能。这些函数可以从Angular的任何组件调用，如控制器、指令等。这有助于将Web应用程序划分为可以重复使用的小的不同逻辑单元。

盘点：关于Angular Js的最新面试题

例如，您的控制器负责数据流并将模型绑定到视图。Angular应用程序可以有多个控制器，以获取整个应用程序所需的数据。从控制器向服务器调用AJAX是多余的，因为每个控制器都将使用类似的代码来调用相同的数据。在这种情况下，使用服务非常有用，因为我们可以编写一个包含代码的服务，从服务器获取数据并将服务注入控制器。服务将具有拨打呼叫的功能。我们可以在控制器中使用服务的这些功能，并调用服务器，这样我们就不需要再次编写相同的代码，它也可以在控制器以外的组件中使用。此外，控制器不再需要执行获取数据的任务，因为服务会处理这一点，从而实现了关注分离的目标。

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

# 如何编写一个指令 @Directive()

```js
import { Directive, OnInit } from '@angular/core';

import { LoggerService } from './logger.service';

let nextId = 1;

@Directive()
export class PeekABooDirective implements OnInit {
  constructor(private logger: LoggerService) { }

  // implement OnInit's `ngOnInit` method
  ngOnInit() {
    this.logIt(`OnInit`);
  }

  logIt(msg: string) {
    this.logger.log(`#${nextId++} ${msg}`);
  }
}
```

# 依赖注入 @Injectable(), providers

```js
import { Injectable } from '@angular/core';

@Injectable()
export class LoggerService {
  logs: string[] = [];
  prevMsg = '';
  prevMsgCount = 1;

  log(msg: string)  {
    if (msg === this.prevMsg) {
      // Repeat message; update last log entry with count.
      this.logs[this.logs.length - 1] = msg + ` (${this.prevMsgCount += 1}x)`;
    } else {
      // New message; log it.
      this.prevMsg = msg;
      this.prevMsgCount = 1;
      this.logs.push(msg);
    }
  }

  clear() { this.logs = []; }

  // schedules a view refresh to ensure display catches up
  tick() {  this.tick_then(() => { }); }
  tick_then(fn: () => any) { setTimeout(fn, 0); }
}
```

```js
import { Component } from '@angular/core';

import { LoggerService } from './logger.service';

@Component({
  selector: 'peek-a-boo-parent',
  template: `
  <hr />
  <div class="parent">
    <h2>Peek-A-Boo</h2>

    <button (click)="toggleChild()">
      {{hasChild ? 'Destroy' : 'Create'}} PeekABooComponent
    </button>
    <button (click)="updateHero()" [hidden]="!hasChild">Update Hero</button>

    <div class="info">
      <peek-a-boo *ngIf="hasChild" [name]="heroName"></peek-a-boo>

      <h3>Lifecycle Hook Log</h3>
      <div *ngFor="let msg of hookLog" class="log">{{msg}}</div>
    </div>
  </div>
  `,
  providers:  [ LoggerService ]
})
export class PeekABooParentComponent {

  hasChild = false;
  hookLog: string[] = [];

  heroName = 'Windstorm';
  private logger: LoggerService;

  constructor(logger: LoggerService) {
    this.logger = logger;
    this.hookLog = logger.logs;
  }

  toggleChild() {
    this.hasChild = !this.hasChild;
    if (this.hasChild) {
      this.heroName = 'Windstorm';
      this.logger.clear(); // clear log on create
    }
    this.hookLog = this.logger.logs;
    this.logger.tick();
  }

  updateHero() {
    this.heroName += '!';
    this.logger.tick();
  }
}

```

# typescript


