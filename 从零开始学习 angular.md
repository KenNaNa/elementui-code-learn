# angular组件以及组件里面的模板

```html
<h1>
  {{title}}
</h1>
<div>
  1+1={{1+1}}
</div>

<!-- 绑定 HTML -->
<div [innerHTML]="h"></div>

<!-- 绑定属性 -->
<div [id]="id"
     [title]="msg">调试工具看看我的属性</div>

<!-- 普通循环 -->
<ul>
  <li *ngFor="let item of list">
    {{item}}
  </li>
</ul>

<!-- 循环的时候，设置 key -->
<ul>
  <li *ngFor="let item of list; let i = index">
    {{item}}--{{i}}
  </li>
</ul>

<!-- template 循环数据 -->
<!-- <ul>
  <li template="ngFor let item of list">
    {{item}}
  </li>
</ul> -->

<!-- 条件判断 -->
<p *ngIf="list.length > 3">这是 ngIF 判断是否显示</p>
<p template="ngIf list.length > 3">这是 ngIF 判断是否显示</p>


<!-- switch 判断 -->
<ul [ngSwitch]="score">
  <li *ngSwitchCase="1">已支付</li>
  <li *ngSwitchCase="2">订单已经确认</li>
  <li *ngSwitchCase="3">已发货</li>
  <li *ngSwitchDefault>无效</li>
</ul>


<!-- 执行点击事件 -->
<button class="button"
        (click)="getData()">点击事件</button>

<button class="button"
        (click)="setData()">设置数据</button>

<!-- 表单事件 -->
<input type="text"
       (keyup)="keyupFn($event)" />

<!-- 双向数据绑定（重要） -->
<input type="text"
       [(ngModel)]="inputValue" /> {{inputValue}}


<!-- class 绑定 -->
<div [ngClass]="{'red': true, 'blue': false}">
  这是一个 div
</div>

public flag=false;
<div [ngClass]="{'red': flag, 'blue': !flag}">
  这是一个 div
</div>

public arr = [1, 3, 4, 5, 6];
<ul>
  <li *ngFor="let item of arr, let i = index">
    <span [ngClass]="{'red': i==0}">{{item}}</span>
  </li>
</ul>

<!-- style 绑定 -->
<div [ngStyle]="{'background-color':'green'}">你好 ngStyle</div>
public attr='red';

<div [ngStyle]="{'background-color':attr}">你好 ngStyle</div>

<!-- 管道 -->
<p>{{today | date:'yyyy-MM-dd HH:mm:ss' }}</p>

```

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.scss']
})
export class HeaderComponent implements OnInit {
  title: string = "标题"
  h: string
  id: string = "这是 id"


  msg: string = "这是 msg"

  list: Array<string> = ['小仙女', '小仙女我爱你', '爱你，爱你，爱你哦']

  inputValue: string = "K 爱小仙女"

  score: number = 1

  arr: Array<string> = this.list

  flag: boolean = true

  attr: string = 'red'


  today: Date = new Date()
  constructor() {
    this.h = "<h2>这是一个 h2 用[innerHTML]来解析</h2>"
  }

  ngOnInit(): void {
  }

  getData() { /*自定义方法获取数据*/
    //获取
    alert(this.msg);
  }

  setData() {
    //设置值
    this.msg = '这是设置的值';
  }

  keyupFn(event) {
    console.log(event)
  }

}
```
