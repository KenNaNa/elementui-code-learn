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
# 管道运算符

```js
import { HttpClient } from '@angular/common/http';
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'fetch',
  pure: false
})
export class FetchJsonPipe implements PipeTransform {
  private cachedData: any = null;
  private cachedUrl = '';

  constructor(private http: HttpClient) { }

  transform(url: string): any {
    if (url !== this.cachedUrl) {
      this.cachedData = null;
      this.cachedUrl = url;
      this.http.get(url).subscribe(result => this.cachedData = result);
    }

    return this.cachedData;
  }
}
```

# 组件导出导入

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';


import { AppComponent } from './app.component';
import { SizerComponent } from './sizer/sizer.component';
import { FormsModule } from '@angular/forms';


@NgModule({
  declarations: [
    AppComponent,
    SizerComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

# 双向绑定

```html
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-sizer',
  templateUrl: './sizer.component.html',
  styleUrls: ['./sizer.component.css']
})
export class SizerComponent {

  @Input()  size!: number | string;
  @Output() sizeChange = new EventEmitter<number>();

  dec() { this.resize(-1); }
  inc() { this.resize(+1); }

  resize(delta: number) {
    this.size = Math.min(40, Math.max(8, +this.size + delta));
    this.sizeChange.emit(this.size);
  }
}


<div>
  <button (click)="dec()" title="smaller">-</button>
  <button (click)="inc()" title="bigger">+</button>
  <label [style.font-size.px]="size">FontSize: {{size}}px</label>
</div>
```

# 内置指令

```html
<h1>Built-in Directives</h1>

<h2>Built-in attribute directives</h2>

<h3 id="ngModel">NgModel (two-way) Binding</h3>

<fieldset><h4>NgModel examples</h4>
  <p>Current item name: {{currentItem.name}}</p>
  <p>
    <label for="without">without NgModel:</label>
    <input [value]="currentItem.name" (input)="currentItem.name=getValue($event)" id="without">
  </p>

  <p>
    <label for="example-ngModel">[(ngModel)]:</label>
    <input [(ngModel)]="currentItem.name" id="example-ngModel">
  </p>

  <p>
    <label for="example-bindon">bindon-ngModel: </label>
    <input bindon-ngModel="currentItem.name" id="example-bindon">
  </p>

  <p>
    <label for="example-change">(ngModelChange)="...name=$event":</label>
    <input [ngModel]="currentItem.name" (ngModelChange)="currentItem.name=$event" id="example-change">
  </p>

  <p>
    <label for="example-uppercase">(ngModelChange)="setUppercaseName($event)"
      <input [ngModel]="currentItem.name" (ngModelChange)="setUppercaseName($event)" id="example-uppercase">
    </label>
  </p>
</fieldset>

<hr><h2 id="ngClass">NgClass Binding</h2>

<p>currentClasses is {{currentClasses | json}}</p>
<div [ngClass]="currentClasses">This div is initially saveable, unchanged, and special.</div>
<ul>
  <li>
    <label for="saveable">saveable</label>
    <input type="checkbox" [(ngModel)]="canSave" id="saveable">
  </li>
  <li>
    <label for="modified">modified:</label>
    <input type="checkbox" [value]="!isUnchanged" (change)="isUnchanged=!isUnchanged" id="modified"></li>
  <li>
    <label for="special">special: <input type="checkbox" [(ngModel)]="isSpecial" id="special"></label>
</li>
</ul>
<button (click)="setCurrentClasses()">Refresh currentClasses</button>

<div [ngClass]="currentClasses">
  This div should be {{ canSave ? "": "not"}} saveable,
                  {{ isUnchanged ? "unchanged" : "modified" }} and
                  {{ isSpecial ? "": "not"}} special after clicking "Refresh".</div>
<br><br>
<!-- toggle the "special" class on/off with a property -->
<div [ngClass]="isSpecial ? 'special' : ''">This div is special</div>
<div class="helpful study course">Helpful study course</div>
<div [ngClass]="{'helpful':false, 'study':true, 'course':true}">Study course</div>


<!-- NgStyle binding -->
<hr><h3>NgStyle Binding</h3>
<div [style.font-size]="isSpecial ? 'x-large' : 'smaller'">
  This div is x-large or smaller.
</div>

<h4>[ngStyle] binding to currentStyles - CSS property names</h4>
<p>currentStyles is {{currentStyles | json}}</p>

<div [ngStyle]="currentStyles">
  This div is initially italic, normal weight, and extra large (24px).
</div>



<br>
<label>italic: <input type="checkbox" [(ngModel)]="canSave"></label> |
<label>normal: <input type="checkbox" [(ngModel)]="isUnchanged"></label> |
<label>xlarge: <input type="checkbox" [(ngModel)]="isSpecial"></label>
<button (click)="setCurrentStyles()">Refresh currentStyles</button>
<br><br>
<div [ngStyle]="currentStyles">
  This div should be {{ canSave ? "italic": "plain"}},
                  {{ isUnchanged ? "normal weight" : "bold" }} and,
                  {{ isSpecial ? "extra large": "normal size"}} after clicking "Refresh".</div>

<hr>
<h2>Built-in structural directives</h2>
<h3 id="ngIf">NgIf Binding</h3>
<div>
  <p>If isActive is true, app-item-detail will render: </p>
  <app-item-detail *ngIf="isActive" [item]="item"></app-item-detail>

  <button (click)="isActiveToggle()">Toggle app-item-detail</button>
</div>
<p>If currentCustomer isn't null, say hello to Laura:</p>
<div *ngIf="currentCustomer">Hello, {{currentCustomer.name}}</div>
<p>nullCustomer is null by default. NgIf guards against null. Give it a value to show it:</p>
<div *ngIf="nullCustomer">Hello, <span>{{nullCustomer}}</span></div>
<button (click)="giveNullCustomerValue()">Give nullCustomer a value</button>


<h4>NgIf binding with template (no *)</h4>

<ng-template [ngIf]="currentItem">Add {{currentItem.name}} with template</ng-template>
<hr>

<h4>Show/hide vs. NgIf</h4>
<!-- isSpecial is true -->
<div [class.hidden]="!isSpecial">Show with class</div>
<div [class.hidden]="isSpecial">Hide with class</div>

<p>ItemDetail is in the DOM but hidden</p>
<app-item-detail [class.hidden]="isSpecial"></app-item-detail>

<div [style.display]="isSpecial ? 'block' : 'none'">Show with style</div>
<div [style.display]="isSpecial ? 'none'  : 'block'">Hide with style</div>


<hr>
<h2 id="ngFor">NgFor Binding</h2>

<div class="box">
  <div *ngFor="let item of items">{{item.name}}</div>
</div>

<p>*ngFor with ItemDetailComponent element</p>
<div class="box">
  <app-item-detail *ngFor="let item of items" [item]="item"></app-item-detail>
</div>


<h4 id="ngFor-index">*ngFor with index</h4>
<p>with <i>semi-colon</i> separator</p>
<div class="box">
  <div *ngFor="let item of items; let i=index">{{i + 1}} - {{item.name}}</div>
</div>

<p>with <i>comma</i> separator</p>
<div class="box">
 <div *ngFor="let item of items, let i=index">{{i + 1}} - {{item.name}}</div>
</div>

<h4 id="ngFor-trackBy">*ngFor trackBy</h4>
<button (click)="resetList()">Reset items</button>
<button (click)="changeIds()">Change ids</button>
<button (click)="clearTrackByCounts()">Clear counts</button>

<p><i>without</i> trackBy</p>
<div class="box">
  <div #noTrackBy *ngFor="let item of items">({{item.id}}) {{item.name}}</div>

  <div id="noTrackByCnt" *ngIf="itemsNoTrackByCount" >
    Item DOM elements change #{{itemsNoTrackByCount}} without trackBy
  </div>
</div>

<p>with trackBy</p>
<div class="box">
  <div #withTrackBy *ngFor="let item of items; trackBy: trackByItems">({{item.id}}) {{item.name}}</div>

  <div id="withTrackByCnt" *ngIf="itemsWithTrackByCount">
    Item DOM elements change #{{itemsWithTrackByCount}} with trackBy
  </div>
</div>

<br><br><br>

<p>with trackBy and <i>semi-colon</i> separator</p>
<div class="box">
  <div *ngFor="let item of items; trackBy: trackByItems">
    ({{item.id}}) {{item.name}}
  </div>
</div>

<p>with trackBy and <i>comma</i> separator</p>
<div class="box">
  <div *ngFor="let item of items, trackBy: trackByItems">({{item.id}}) {{item.name}}</div>
</div>

<p>with trackBy and <i>space</i> separator</p>
<div class="box">
  <div *ngFor="let item of items trackBy: trackByItems">({{item.id}}) {{item.name}}</div>
</div>

<p>with <i>generic</i> trackById function</p>
<div class="box">
  <div *ngFor="let item of items, trackBy: trackById">({{item.id}}) {{item.name}}</div>
</div>

<hr><h2>NgSwitch Binding</h2>

<p>Pick your favorite item</p>
<div>
  <label *ngFor="let i of items">
    <div><input type="radio" name="items" [(ngModel)]="currentItem" [value]="i">{{i.name}}
    </div>
  </label>
</div>

<div [ngSwitch]="currentItem.feature">
  <app-stout-item    *ngSwitchCase="'stout'"    [item]="currentItem"></app-stout-item>
  <app-device-item   *ngSwitchCase="'slim'"     [item]="currentItem"></app-device-item>
  <app-lost-item     *ngSwitchCase="'vintage'"  [item]="currentItem"></app-lost-item>
  <app-best-item     *ngSwitchCase="'bright'"   [item]="currentItem"></app-best-item>
  <div *ngSwitchCase="'bright'"> Are you as bright as {{currentItem.name}}?</div>
  <app-unknown-item  *ngSwitchDefault           [item]="currentItem"></app-unknown-item>
</div>
```

# 路由的注册

### 根路由注册

```js
import { RouterModule, Routes } from '@angular/router';
import { PageNotFoundComponent } from './page-not-found/page-not-found.component';
import { CrisisListComponent } from './crisis-list/crisis-list.component';
import { HeroListComponent } from './hero-list/hero-list.component';

const appRoutes: Routes = [
  { path: '', redirectTo: '/heroes', pathMatch: 'full' },
  { path: 'crisis-center', component: CrisisListComponent },
  { path: 'heroes', component: HeroListComponent },
  { path: '**', component: PageNotFoundComponent }
]

@NgModule({
  declarations: [
    PageNotFoundComponent,
    CrisisListComponent,
    HeroListComponent
  ],
  imports: [
    CommonModule,
    RouterModule.forRoot(
      appRoutes,
      { enableTracing: true } // <-- debugging purposes only
    )
  ],
  exports: [
    RouterModule
  ]
})
export class AppRoutingModule { }
```

# typescript


