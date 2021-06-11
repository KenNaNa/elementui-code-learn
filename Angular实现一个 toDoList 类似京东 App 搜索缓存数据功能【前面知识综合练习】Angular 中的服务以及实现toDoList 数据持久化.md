# 实例一

search.compoent.html

```html
<h2>京东搜索</h2>
<div class="search">
  <input type="text"
         [(ngModel)]="keyword"><button (click)="doSearch()"
          class="button1">搜索</button>
  <hr>
  <ul>
    <li *ngFor="let item of historyList;let key=index;">{{item}}-------<button (click)="deleteHistory(key)">删除</button>
    </li>
  </ul>
</div>
```

search.component.ts

```ts
import { Component, OnInit } from '@angular/core';
//引入服务,需要注意路径
import { StorageService } from '../../services/storage.service';


@Component({
  selector: 'app-search',
  templateUrl: './search.component.html',
  styleUrls: ['./search.component.scss']
})
export class SearchComponent implements OnInit {
  keyword: string;
  historyList: Array<any> = [];
  constructor(public storage: StorageService) { }

  ngOnInit(): void {
    console.log('页面刷新会触发这个生命周期函数');
    let searchList = this.storage.get('searchlist');
    if (searchList) {
      this.historyList = searchList;
    }
  }

  doSearch() {
    //if语句检查输入的值在列表中是否存在，如果=-1说明不存在，则需要push进去
    if (this.historyList.indexOf(this.keyword) == -1) {
      this.historyList.push(this.keyword);//理解push是什么意思
    }

    this.keyword = '';
    //获取属性的值
    console.log(this.keyword);

    //缓存数据,实现数据持久化
    this.storage.set('searchlist', this.historyList);
  }

  //删除历史记录函数
  deleteHistory(key) {
    //alert(key);
    this.historyList.splice(key, 1);//splice可以在数组里删除，增加，修改一个值。在这里表示从key位置往后删除一个值。
    this.storage.set('searchlist', this.historyList);
  }

}
```

search.component.scss

```scss
.search {
  width: 400px;
  margin: 20px auto;
}

input {
  margin-bottom: 20px;
  width: 300px;
  height: 32px;
}

.button1 {
  width: 60px;
  height: 35px;
  margin-left: 10px;
}

h2 {
  /* 标题居中 */
  text-align: center;
}
```

storage.service.ts

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class StorageService {

  constructor() {

  }
  // set 数据
  set(key: string, value: any) {
    localStorage.setItem(key, JSON.stringify(value));
  }
  // get 数据
  get(key: string) {
    return JSON.parse(localStorage.getItem(key))
  }
  // remove 数据
  remove(key: string) {
    localStorage.removeItem(key)
  }
}
```
todolist.component.html

```html
<h2>todolist</h2>
<div class="todolist">
  <input class="form_input"
         type="text"
         [(ngModel)]="keyword"
         (keyup)="doAdd($event)" />
  <hr>
  <h3>待办事项</h3>
  <ul>
    <li *ngFor="let item of todolist;let key=index;"
        [hidden]="item.status==1">
      <!-- (change)表示触发某个事件 -->
      <input type="checkbox"
             [(ngModel)]="item.status"
             (change)="checkboxChange()" />{{item.title}} --------<button (click)="deleteData(key)">X</button>
    </li>
  </ul>

  <h3>已完成事项</h3>
  <ul>
    <li *ngFor="let item of todolist;let key=index;"
        [hidden]="item.status==0">
      <input type="checkbox"
             [(ngModel)]="item.status"
             (change)="checkboxChange()" />{{item.title}} --------<button (click)="deleteData(key)">X</button>
    </li>
  </ul>

</div>
```
todolist.component.scss

```scss
h2 {
  /* 标题居中 */
  text-align: center;

}

.todolist {
  width: 400px;
  margin: 20px auto;
}

.form_input {
  margin-bottom: 20px;
  width: 300px;
  height: 32px;
}

li {
  /* 行高 */
  line-height: 40px;
}
```
todolist.component.ts

```ts
import { Component, OnInit } from '@angular/core';
//引入服务,需要注意路径
import { StorageService } from '../../services/storage.service';
@Component({
  selector: 'app-todolist',
  templateUrl: './todolist.component.html',
  styleUrls: ['./todolist.component.scss']
})
export class TodolistComponent implements OnInit {
  keyword: string;
  todolist: Array<any> = [];
  constructor(public storage: StorageService) { }

  ngOnInit(): void {
    console.log('页面刷新会触发这个生命周期函数');
    var todolist: any = this.storage.get('todolist');
    if (todolist) {
      this.todolist = todolist;
    }
  }

  doAdd(e) {
    if (e.keyCode === 13) {
      if (!this.todolistHasKeyword(this.todolist, this.keyword)) {
        this.todolist.push(
          {
            title: this.keyword,
            status: 0
          }
        )
        this.keyword = ''
      } else {
        alert('数据已经存在')
        this.keyword = ''
      }
    }
    this.storage.set('todolist', this.todolist);
  }

  // 删除函数
  deleteData(key) {
    this.todolist.splice(key, 1);//splice可以在数组里删除，增加，修改一个值。在这里表示从key位置往后删除一个值。
    this.storage.set('todolist', this.todolist);
  }

  //如果数组里面有keyword返回true，否则返回false
  todolistHasKeyword(todolist: any, keyword: any) {

    //异步 会存在问题
    /*todolist.forEach(value => {
      if (value.title==keyword) {
        return true;
      }
    });
    */

    //另一种方法，用到for循环
    if (!keyword) return false;
    for (var i = 0; i < todolist.length; i++) {
      if (todolist[i].title == keyword) {
        return true;
      }
    }
    return false;

  }

  checkboxChange() {
    console.log('事件触发了');
    this.storage.set('todolist', this.todolist);//用到this一定要注意this的指向
  }

}
```


