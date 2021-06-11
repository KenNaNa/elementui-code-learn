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

# Angular表单（用一个实例说明）

```html
<h2>人员登记系统</h2>
<div class="people_list">
  <!-- 都用到了双向数据绑定 -->
  <ul>
    <li>
      <!-- value和class在css里的用法一样 -->
      姓 名：<input type="text"
             id="username"
             [(ngModel)]="peopleInfo.username"
             value="fonm_input" />
    </li>
    <li>
      性 别：<input type="radio"
             value="1"
             name="sex"
             id="sex1"
             [(ngModel)]="peopleInfo.sex" /><label for="sex1">男</label>
      <input type="radio"
             value="2"
             name="sex"
             id="sex2"
             [(ngModel)]="peopleInfo.sex" /><label for="sex1">女</label>
    </li>
    <li>
      城 市：
      <select name="city"
              id="city"
              [(ngModel)]="peopleInfo.city">
        <option [value]="item"
                *ngFor="let item of peopleInfo.cityList">{{item}}</option>
      </select>

    </li>
    <li>
      爱 好：
      <span *ngFor="let item of peopleInfo.hobby;let key=index;">
        <!-- 绑定动态属性  用到了一个拼接字符串-->
        <input type="checkbox"
               [id]="'check'+key"
               [(ngModel)]="item.checked" /><label [for]="'check'+key">{{item.title}}</label>
        &nbsp;&nbsp;
      </span>
    </li>
    <li>
      备注：
      <textarea name="mark"
                id="mark"
                cols="30"
                rows="10"
                [(ngModel)]="peopleInfo.mark"></textarea>

    </li>
  </ul>
  <button (click)="doSubmit()"
          class="submit">获取表单的内容</button>
  <!-- json使用到了管道的知识 -->
  <pre class="c">{{peopleInfo |json}}</pre>

</div>

h2 {
  text-align: center;
  margin: 10px auto;
}

.people_list {
  width: 400px;
  margin: 40px auto;
  padding: 20px;
  border: 1px solid #eee;
}

li {
  height: 50px;
  line-height: 50px;
}

.fonm_input {
  width: 300px;
  height: 30px;
}

.submit {
  height: 30px;
  float: right;
  margin-right: 50px;
  margin-top: 120px;
}

.c {
  margin-top: 120px;
}
```

```ts
import { Component, OnInit } from '@angular/core';
type ItemType = {
  title?: string
  checked?: boolean
}

interface PeopleType {
  username?: string;
  sex?: string;
  cityList?: Array<string>;
  city?: string;
  hobby?: Array<ItemType>;
  mark?: string;
}


@Component({
  selector: 'app-form',
  templateUrl: './form.component.html',
  styleUrls: ['./form.component.scss']
})
export class FormComponent implements OnInit {
  // 定义一个对象，
  public peopleInfo: PeopleType = {
    username: '',
    sex: '1',
    cityList: ['北京', '上海', '深圳', '杭州', '潍坊', '青岛', '厦门'],
    city: '北京',
    hobby: [
      {
        title: '吃饭',
        checked: false
      }, {
        title: '睡觉',
        checked: false
      }, {
        title: '看剧',
        checked: false
      }, {
        title: '敲代码',
        checked: true
      }
    ],
    mark: ''
  }
  constructor() { }

  ngOnInit(): void {
  }
  // 自定义方法
  doSubmit() {
    console.log(this.peopleInfo)
  }
}
```

