transition.component.html

```html
<div>
  内容区域
  <button (click)="showAside()">弹出侧边栏</button>
  <button (click)="hiddenAside()">隐藏侧边栏</button>
</div>
<aside id="aside">

  这是一个侧边栏
</aside>

```

transition.component.scss

```scss
#aside {
  width: 180px;
  height: 100%;
  /* 绝对定位 */
  position: absolute;
  right: 0px;
  top: 0px;
  background: #000;
  color: #fff;
  transform: translate(100%, 0);
  /* 设置动画的过渡 */
  transition: all 2s;
}
```

transition.component.ts

```ts
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-transition',
  templateUrl: './transition.component.html',
  styleUrls: ['./transition.component.scss']
})
export class TransitionComponent implements OnInit {
  flag: boolean = true;
  constructor() { }

  ngOnInit(): void {
  }
  //自定义方法
  showAside() {
    //用原生js获取dom节点,用dom节点来改变一下style样式
    var asideDom: HTMLElement = document.getElementById('aside');
    asideDom.style.transform = "translate(0,0)";
  }

  hiddenAside() {
    //用原生js获取dom节点,用dom节点来改变一下style样式
    var asideDom: HTMLElement = document.getElementById('aside');
    asideDom.style.transform = "translate(100%,0)";
  }

}
```
