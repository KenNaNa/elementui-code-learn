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

```ts
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";
import { TestComponent } from "./test/test.component";
import { ChartsRoutingModule } from "./charts-routing.module";
import { TiTabModule } from "@cloud/tiny3";

@NgModule({
  declarations: [TestComponent],
  imports: [CommonModule, ChartsRoutingModule, TiTabModule],
})
export class ChartsModule {}
```

路由跳转

7. ERROR TypeError: Cannot read property 'nativeElement' of undefined

https://stackoverflow.com/questions/39158922/viewchild-not-working-cannot-read-property-nativeelement-of-undefined


8. ng g c 组件路径 创建组件


echarts 总结

1. x 轴间隔问题, 内容倾斜
```ts
axisLabel: {
  interval: 0,
  rotate:40
},
```

2. Can't bind to 'ngModel' since it isn't a known property of 'ti-date'

https://stackoverflow.com/questions/43298011/angular-error-cant-bind-to-ngmodel-since-it-isnt-a-known-property-of-inpu
```ts
import { FormsModule } from '@angular/forms';

@NgModule({
    imports: [
         FormsModule      
    ]
})
```

3. angular 如何格式化日期

https://blog.csdn.net/u011763994/article/details/78747587

4. ncaught (in promise): NullInjectorError: R3InjectorError(ChartsModule)[DatePipe -> DatePipe -> DatePipe -> DatePipe]: 
  NullInjectorError: No provider for DatePipe!


5. ERROR Error: InvalidPipeArgument: 'The minimum number of digits after fraction (2) is higher than the maximum (1).' for pipe 'DecimalPipe'

6. echarts 鼠标放上去显示提示框属性详解！

https://blog.csdn.net/zhen_xin_ai_ni/article/details/84103026


1. angular 如何处理数据小数点保留一位

接收的参数格式为{最少整数位数}.{最少小数位数}-{最多小数位数} 

2. 处理日期公共的方法

```ts
import { Injectable } from "@angular/core";
import { DatePipe } from "@angular/common";
@Injectable({
  providedIn: "root",
})
export class DateService {
  constructor(private datePipe: DatePipe) {}

  getFullYear(value: Date): string {
    return `${new Date(value).getFullYear()}`;
  }

  getYearMonth(value: Date): string {
    return this.datePipe.transform(value, "yyyy-MM");
  }

  genBefore12Month(value: Date): Array<string> {
    let dateArr = [];
    const date = new Date(value);
    date.setMonth(date.getMonth() + 1, 1); //获取到当前月份,设置月份
    for (let i = 0; i < 12; i++) {
      date.setMonth(date.getMonth() - 1); //每次循环一次 月份值减1
      let m: string | number = date.getMonth() + 1;
      m = this.padZero(m);
      dateArr.push(date.getFullYear() + "-" + m);
      dateArr = dateArr.sort((a, b) => {
        return new Date(a).getTime() - new Date(b).getTime();
      });
    }
    return dateArr;
  }

  genLessNowMonth(value: Date): string {
    const date = new Date(value);
    date.setMonth(date.getMonth() - 12, 1);
    let m: string | number = date.getMonth() + 1;
    m = this.padZero(m);
    return date.getFullYear() + "-" + m;
  }

  genMoreNowMonth(value: Date): string {
    const date = new Date(value);
    date.setMonth(date.getMonth() + 1, 1);
    let m: string | number = date.getMonth() + 1;
    m = this.padZero(m);
    return date.getFullYear() + "-" + m;
  }

  padZero(value): string {
    return value < 10 ? "0" + value : value;
  }
}
```

1. 如何切换分支

查看分支：git branch -a, git branch
git pull
切换分支： git checkout 分支名字

git status 查看状态
git add 添加修改文件
git commit 提交本地仓库
git push 提交到远程仓库


提交 mr 提交合并请求

需求单号

RR2021050600855

2. 图片如何自适应宽度

<img src="" width="auto"  height="100%">

https://segmentfault.com/a/1190000018279674

3. 像这种顶部导航栏，左边菜单栏最好不要使用顶部布局，慎重使用 display: flex 布局

4. 两栏布局

可以解决出现滚动条的时候，有一部分空白的地方，html {width: fit-content; min-width: 1920px;}

5. 分析

事件 region 分布

P几/NRI
P几/CRI

6. angular4数据改变，页面没有渲染解决方法

https://blog.csdn.net/csdn277/article/details/85288774

7. 如何自适应屏幕宽度

Overload 1 of 2, '(this: GlobalEventHandlers, ev: UIEvent): any', gave the following error.
        Argument of type '(e: any) => void' is not assignable to parameter of type 'UIEvent'.
          Type '(e: any) => void' is missing the following properties from type 'UIEvent': detail, view, which, bubbles, and 21 more.
      Overload 2 of 2, '(this: Window, ev: UIEvent): any', gave the following error.
        Argument of type '(e: any) => void' is not assignable to parameter of type 'UIEvent'.

https://blog.csdn.net/m0_37729058/article/details/78673510

8. js 获取屏幕宽度

https://www.jianshu.com/p/1a82275d52c8

9. Type boolean trivially inferred from a boolean literal, remove type annotation.

10. Disallows explicit type declarations for variables or parameters initialized to a number, string, or boolean (no-inferrable-types)

https://github.com/typescript-eslint/typescript-eslint/blob/v2.33.0/packages/eslint-plugin/docs/rules/no-inferrable-types.md

11. Disallow aliasing this (no-this-alias)

https://github.com/typescript-eslint/typescript-eslint/blob/v2.33.0/packages/eslint-plugin/docs/rules/no-this-alias.md

12. 解决浏览器 resize 

```ts
  ngAfterViewInit(): void {
    this.resize();
    window.addEventListener("resize", () => this.resize(), false);
  }

  resize() {
    (document.documentElement || document.body).style.width =
      window.innerWidth + "px";
    (document.documentElement || document.body).style.height =
      window.innerHeight + "px";
    (document.documentElement || document.body).style.minWidth =
      (document.documentElement || document.body).clientWidth + "px";
  }
```

js 获取的是 window 的分辨率，并不能获取他实际的物体的宽度

git commit 中文乱码

https://segmentfault.com/q/1010000020503297/a-1020000020510180

git config --global i18n.commitencoding utf-8

1. 更新报表时间，报表名称去掉，权限管理，编辑下拉

2. 合并多个 commit : git rebase

rebase 用法小结

https://www.jianshu.com/p/4a8f4af4e803

https://www.huaweicloud.com/articles/bcae81b31451dc4c287fc123ccc1da94.html

3. angular 表单验证

https://www.jianshu.com/p/6f511c5fefe5

4. ERROR Error: There is no FormControl instance attached to form control element with name: 'urlTimeControl'

创建表单控制器

1. 从 @angular/forms 导入 FormBuilder, FormControl, FormGroup

2. 注入 private formBuilder: FormBuilder

3. public menuConfigForm: FormGroup;

4. new FormControl()

5. formGroup 有 removeControl, addControl, get, reset， 



类型“EventTarget”上不存在属性“children”。ts(2339)

```ts
  setListener() {
    document.body.addEventListener(
      "mousewheel",
      (e) => {
        const element = (e.currentTarget as HTMLElement)
          .children[8] as HTMLElement;
        if (element) {
          const display = ((e.currentTarget as HTMLElement)
            .children[8] as HTMLElement).style.display;
          if (display === "none") {
            ((e.currentTarget as HTMLElement)
              .children[8] as HTMLElement).style.display = "block";
          }
        }
      },
      true
    );
  }
```

git commit -m"RR2021050600855 调试退出登录退出不了"

```ts
import { Injectable } from "@angular/core";

@Injectable({
  providedIn: "root",
})
export class CookieService {
  constructor() {}

  // eslint-disable-next-line max-params
  static setItem(
    key: string,
    value: string,
    ttl?: number,
    path?: string
  ): void {
    let cookieString = `${key}=${encodeURIComponent(value)}`;
    if (ttl > 0) {
      const date = new Date();
      date.setTime(date.getTime() + ttl);
      cookieString += `; expires=${date.toUTCString()}; path=${path || "/"}`;
    }
    document.cookie = cookieString;
  }

  static getItem(key: string): string {
    const cookies = document.cookie.split(";");

    for (let i = 0, len = cookies.length; i < len; i += 1) {
      const arr = cookies[i].split("=");
      if (arr[0].trim() === key.trim()) {
        return decodeURIComponent(arr[1]);
      }
    }

    return "";
  }

  static removeItem(key: string, path?: string): void {
    const date = new Date(
      new Date().getTime() - 10 * 24 * 3600000
    ).toUTCString();

    document.cookie = `${key}=; expires=${date}; path=${path || "/"}`;
  }

  static removeAll(): void {
    const keys = document.cookie.match(/[^ =;]+(?=\=)/g);
    if (keys) {
      for (var i = keys.length; i--; )
        document.cookie = keys[i] + "=0;expires=" + new Date(0).toUTCString();
    }
  }
}

```

JS 无法清除Cookie的解决方法 #12

https://github.com/xieshanshan/blog/issues/12

1. this.router.navigateByUrl("/management/authority/application"); 不会跳转过去

2. this.router.navigate(["/management/authority/application"])

this.router.navigate 不会自动刷新页面

https://blog.csdn.net/weixin_42080477/article/details/97662849

Angular刷新当前页面的几种方法（转载）

https://blog.csdn.net/xuehu837769474/article/details/104763685/?utm_term=angular%E5%88%B7%E6%96%B0%E9%A1%B5%E9%9D%A2%E9%97%AE%E9%A2%98&utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduweb~default-0-104763685&spm=3001.4430

5. 监听路由变化

Angular8 中 监听url变化
https://blog.csdn.net/weixin_41844140/article/details/106802253


6. 如何 rebase git 冲突

别人和我改动的代码相同，但是别人先合进主分支

git checkout 主分支

git pull 更新主分支最新代码

git checkout 自己的分支

git rebase 主分支

解决冲突，git add 冲突的文件，git rebase --continue 

git commit -m""

git push


8. 数据加载不出来，有可能是 DOM 已经加载完成，但是数据还没有过来导致的，所以可以加下  `*ngIf="show"` 来等加载完成之后再展示数据
```ts
<ti-treeselect
  *ngIf="tiTreeSelectRegion.options.length"
  [multiple]="true"
  [searchable]="true"
  [selectAll]="true"
  id="tiTreeSelectRegion"
  placeholder="请选择"
  [options]="tiTreeSelectRegion.options"
  [(ngModel)]="tiTreeSelectRegion.value"
  (ngModelChange)="tiTreeSelectRegion.ngModelChange($event)"
>
</ti-treeselect>
```

9. ERROR Error: Initialize failed: invalid dom.

10. 疑问点
1）


pa: [], lo: ['dianxin']

如何修改第三方插件样式

1. `:host` 表示修改当前组件
2. `::ng-deep` 可以忽略中间 className 的嵌套层级关系，直接找到你要修改的 className

NodeInjector: NOT_FOUND [NgControl]

https://stackoverflow.com/questions/48525216/getting-no-provider-for-ngcontrol-error-after-adding-reactiveformsmodule-to-my-a
https://stackoverflow.com/questions/58833589/ngcontrol-in-directive-no-provider-for-ngcontrol

```ts
import { ReactiveFormsModule, FormsModule } from "@angular/forms";
```

https://blog.csdn.net/lvxinaidou/article/details/108041861

````ts
<d-tab [id]="tabs.departRank.key" [title]="tabs.departRank.title">
    <ng-template dTabContent>
      <div class="nf-chart" id="depart_chart_1" #depart_chart_1></div>
      <div class="nf-chart" id="depart_chart_2" #depart_chart_2></div>
      <div class="nf-chart" id="depart_chart_3" #depart_chart_3></div>
      <div class="nf-chart" id="depart_chart_4" #depart_chart_4></div>
      <div class="nf-chart" id="depart_chart_5" #depart_chart_5></div>
      <div class="nf-chart" id="depart_chart_6" #depart_chart_6></div>
      <div class="nf-chart" id="depart_chart_7" #depart_chart_7></div>
      <div class="nf-chart" id="depart_chart_8" #depart_chart_8></div>
      <div class="nf-chart" id="depart_chart_9" #depart_chart_9></div>
      <div class="nf-chart" id="depart_chart_10" #depart_chart_10></div>
      <div class="nf-chart" id="depart_chart_1" #depart_chart_1></div>
      <div class="nf-chart" id="depart_chart_12" #depart_chart_12></div>
    </ng-template>
  </d-tab>
```

Angular学习笔记58：组件的样式设置，样式作用范围，引入样式的方式

https://blog.csdn.net/wjyyhhxit/article/details/100626777

https://angular.cn/guide/component-styles


1. 新用户，没有角色的时候，去申请角色的时候，我们应该怎么处理

2. 表单验证问题

git rebase 合并多个 commit

git rebase -i head~数字

git push -f


3. 调试
4. 后面优化

5. echarts x轴文字显示不全(xAxis文字倾斜比较全面的3种做法值得推荐)

https://blog.csdn.net/kebi007/article/details/68488694

1. 疑问，如果他查询出来的数据没有还要展示？

2. pc端使用rem进行适配 （vu-cli3/4+lib-flexible+postcss-pxtorem）

https://blog.csdn.net/weixin_40297452/article/details/107563825

3. echarts pie饼图中间显示总数 pie饼图类型之间设置白缝隙

https://blog.csdn.net/zhengshaofeng1/article/details/106358494

4. ERROR in Tried to overwrite /data/fuxi_ci_workspace/6108ad7cac264d2b7e7ccf23/web/ng8/node_modules/_@avenueui_ng-devui@10.66.0@@avenueui/ng-devui/node_modules/@angular/cdk/platform/platform.d.ts.__ivy_ngcc_bak with an ngcc back up file, which is disallowed.  

https://stackoverflow.com/questions/60130728/tried-to-overwrite-angular-cdk-stepper-step-header-d-ts-ivy-ngcc-bak-with-an

5. Angular 6 StaticInjectorError (Platform: core) No provider for

https://stackoverflow.com/questions/52288263/angular-6-staticinjectorerror-platform-core-no-provider-for

6. 移动端图表插件

https://chartjs.bootcss.com/

7. echarts 移动端如何

8. 后裔菜单 app/availability/availabilityManagement/overview

9. canvas 如何自适应移动端

https://blog.csdn.net/lm1022/article/details/78211075

10. [object Object] is not a PostCSS plugin

https://stackoverflow.com/questions/40090111/postcss-error-object-object-is-not-a-postcss-plugin

11. 移动端适配

```ts
npm install postcss-pxtorem@5.1.1
npm install autoprefixer@9.7.3

postcss.config.js
module.exports = {
    plugins: [
        require('autoprefixer'),
        require('postcss-pxtorem')({
            rootValue: 37.5, //换算基数， 

            unitPrecision: 3, //允许REM单位增长到的十进制数字,小数点后保留的位数。

            propList: ['*'],

            exclude: /(node_module)/,  //默认false，可以（reg）利用正则表达式排除某些文件夹的方法，例如/(node_module)/ 。如果想把前端UI框架内的px也转换成rem，请把此属性设为默认值

            selectorBlackList: ['.van'], //要忽略并保留为px的选择器，本项目我是用的vant ui框架，所以忽略他

            mediaQuery: false,  //（布尔值）允许在媒体查询中转换px。

            minPixelValue: 1 //设置要替换的最小像素值
        })
    ]
};
```

11. HomeComponent_Host.ngfactory.js? [sm]:1 ERROR NullInjectorError: StaticInjectorError(AppModule)[DatePipe]: 
https://stackoverflow.com/questions/58540498/no-provider-for-datepipe

12. 警戒线

https://blog.csdn.net/weixin_44273026/article/details/105087524

13. 如何解决 echarts 图标模糊问题


1. 不直接使用 canvas 画 echarts，会导致画出来的页面模糊

<canvas id="canvas"></canvas>

2. 直接使用 html 来 画


<div
   id="lineCharts"
   ref="lineCharts"
   style="height: 100%;width: 100%;margin: 0;padding: 0;display: block;"
></div>
```

14. Angular10 路由传参

https://blog.csdn.net/Damien_J_Scott/article/details/115356450






