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


