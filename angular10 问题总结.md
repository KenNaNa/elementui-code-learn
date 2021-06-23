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

# 如何管理项目 API

1. 可以这样使用

```js
const apiUrlConfig = {
  "MIGRATE_RESOURCE": {
    "getMigrateList": ``,
    "getOperateLogList": ``,
    "queryWarnObjectResourceList": ``,
    "getWarnObjectStaticData": ``,
    "clearWarnObjectById": ``,
    "generatSolution": ``,
    "postSingleMigrationOperation": ``
  }
};
export default apiUrlConfig;
```
2. 可以使用 service 的方式

```js
import { Injectable } from '@angular/core';
import { AppService } from '@src/app/app.service';
import { HttpService } from '@src/app/core/net/http.service';
import { Observable } from 'rxjs';

@Injectable()
export class OccdmService {
    private readonly PREFIX_PATH = this.appService.getAppWebPath();
    private occdmUrlConf = {
        _layerUrl: this.PREFIX_PATH + '',
        _projectListUrl: this.PREFIX_PATH + ''
    }

    constructor(private appService: AppService, private http: HttpService) { }

    // 查询层级
    getLayer(options?): Observable<any> {
        const tmpApi = this.occdmUrlConf._layerUrl;
        return this.http.get(tmpApi, options);
    }
    //  查询项目列表
    getProjectList(options?): Observable<any> {
        const tmpApi = this.occdmUrlConf._projectListUrl;
        return this.http.get(tmpApi, options);
    }
}
```

# Can't have multiple template bindings on one element. Use only one attribute prefixed with *

```html
<tr>
  <th
    tiOverflow
    *ngFor="let column of columns; let i = index"
    *ngIf="column.show === true || column.show === undefined"
  >
    <ti-cell-text>{{ columns[i].title }}</ti-cell-text>
    <ti-head-sort
      *ngIf="columns[i].sortKey"
      [sortKey]="columns[i].sortKey"
    ></ti-head-sort>
  </th>
</tr>
```

# Angular error: “Can't bind to 'ngModel' since it isn't a known property of 'input'”
1. If 'ti-select' is an Angular component and it has 'ngModel' input, then verify that it is part of this module.
3. If 'ti-select' is a Web Component then add 'CUSTOM_ELEMENTS_SCHEMA' to the '@NgModule.schemas' of this component to suppress this message.
5. To allow any property add 'NO_ERRORS_SCHEMA' to the '@NgModule.schemas' of this component.

```js
import { FormsModule } from '@angular/forms';

@NgModule({
    imports: [
         FormsModule      
    ]
})
```

# 没有初始化对象，检测不出来

```js
  // 选项配置
  public sourceNameConfig: Record<string, any> = {};
  public operateConfig: Record<string, any> = {};
```
