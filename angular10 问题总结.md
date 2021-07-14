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

# https://limeii.github.io/2019/08/rxjs-notification/

# https://angular.io/guide/http

# 时间过滤

```ts
<td tiOverflow>{{row.deploy_time | date:appService.curDateFormat()}}</td>
```

1. 如何配置跨域问题
2. 如何获取环境问题
3. Error: unsafe value used in a resource URL context

https://stackoverflow.com/questions/37927657/img-unsafe-value-used-in-a-resource-url-context

4. app.component.ts 中的 doInit() 方法需要优化
5. 动态加载路由
6. 国际化
7. 退出登录


8. w3 测试环境 lwx1027489, huawei@123

1. 先研究下多语言模块

charts 模块
1. apiUrl http url
2. services https methods
3. 组件

4. 解决 unsafe value used in a resource URL context

 5. Invalid configuration of route '/charts/test': path cannot start with a slash

```ts
// 安全过滤的管道
import { Pipe, PipeTransform } from "@angular/core";
import { DomSanitizer } from "@angular/platform-browser";
@Pipe({
  name: "safe",
})
export class SafePipe implements PipeTransform {
  constructor(private sanitizer: DomSanitizer) {}
  transform(url) {
    return this.sanitizer.bypassSecurityTrustResourceUrl(url);
  }
}
```

```ts
import { Component, ElementRef, OnInit, ViewChild } from "@angular/core";
import { ChartsService } from "../charts.service";
// 引入echarts
import * as echarts from "echarts/lib/echarts";

// 引入饼图
import "echarts/lib/chart/pie";
import "echarts/lib/component/legend";
import "echarts/lib/component/legend/scrollableLegendAction";
import "echarts/lib/component/legend/ScrollableLegendModel";
import "echarts/lib/component/legend/ScrollableLegendView";
import "echarts/lib/component/title";
import "echarts/lib/component/toolbox";
// 引入提示框和标题组件
import "echarts/lib/component/tooltip";

@Component({
  selector: "app-test",
  templateUrl: "./test.component.html",
  styleUrls: ["./test.component.less"],
})
export class TestComponent implements OnInit {
  // 通过临时模板变量为 echarts 准备一个 dom
  @ViewChild("chart") chartDiv: ElementRef;
  private postParams: Record<string, any> = {
    isCache: 0,
    fields: ["user_department", "count(1)", "sum(role_type)"],
    datasetName: "user_role_t",
    containConditions: {},
    moreConditions: {},
    lessConditions: {},
    equalsConditions: {},
    additions: "group by user_department",
  };
  constructor(private chartsService: ChartsService) {}

  ngOnInit(): void {
    this.chartsService
      .postDataSet({ ...this.postParams })
      .toPromise()
      .then((res) => {
        if (res && res.data) {
          this.initChart(this.handleData(res.data));
        }
      });
  }

  // 处理请求回来的数据
  handleData(data: Array<Record<string, any>>): Array<Record<string, any>> {
    const series = [];
    const count = data.map((item) => {
      return { value: item["count(1)"], name: item.user_department };
    });
    const sum = data.map((item) => {
      return { value: item["sum(role_type)"], name: item.user_department };
    });
    series.push({
      name: "总人数",
      type: "pie",
      radius: [20, 140],
      center: ["30%", "50%"],
      data: count,
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowOffsetX: 0,
          shadowColor: "rgba(0, 0, 0, 0.5)",
        },
      },
    });
    series.push({
      name: "总人次",
      type: "pie",
      radius: [20, 140],
      center: ["75%", "50%"],
      data: sum,
      emphasis: {
        itemStyle: {
          shadowBlur: 10,
          shadowOffsetX: 0,
          shadowColor: "rgba(0, 0, 0, 0.5)",
        },
      },
    });

    return series;
  }

  //初始化饼图
  initChart(data: Array<Record<string, any>>): void {
    // 指定图表的配置项和数据
    const option = {
      tooltip: {
        trigger: "item",
      },
      series: data,
    };
    // 使用刚指定的配置项和数据显示图表。
    echarts.init(this.chartDiv.nativeElement, "light").setOption(option);
  }
}
```

6. 总结
自定义的组件在自己的模块注册  子模块.module.ts、app.module.ts 中的 declarations 注册
第三方组件需要在自己的 子模块.module.ts 里面 declarations 注册，导入模块 imports
test 组件 -> charts.module declarations   注册
charts-routing.module.ts -> 在 charts.module.ts imports 注册
第三方组件模块 -> 在 charts.module.ts imports 注册
