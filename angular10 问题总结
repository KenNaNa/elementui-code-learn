# Can't bind to 'ngForTrack' since it isn't a known property of 'tr'.

# tiMessage 弹不出来，报错

```js
// app.module.ts
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
```

# 页面数据没有及时刷新问题

```js
constructor(private changeDetectorRef: ChangeDetectorRef, private zone: NgZone) {
  this.zone.run(() => {
    // 要更新视图的代码
    this.initFn();
    
    // 更新组件数据
      this.changeDetectorRef.markForCheck();
      this.changeDetectorRef.detectChanges();
  })  
}


```
