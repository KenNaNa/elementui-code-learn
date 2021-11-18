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

```ts
add_start_one_month: "2021-08-01"
add_start_one_year: "2022-07-01"
alarm_find_incident_amount: "0"
alarm_not_find_incident_amount: "0"
change_result_p1p2_amount: "0"
change_result_p1p2p3_amount: "0"
change_result_p3_nri_amount: "0"
change_result_p4_nri_amount: "0"
concat_region: "华北-乌兰察布二零一"
concat_root_cause_dept: "EI服务产品部"
concat_root_cause_full_service: "EI容器框架(ECF)"
concat_root_cause_scope: "Gauss AP服务域项目群"
concat_root_cause_service: "ECF"
customer_find_fault_amount: "1"
etl_date: "2021-07-15 09:58:44"
find_fault_customer_amount: "1"
first_type: "产品问题"
is_cloudbu_incident: "Cloud BU"
location: "内部IT专属云"
p1_amount: "0"
p1_recovery_duration: "0.0"
p1p2_amount: "0"
p1p2_recovery_duration: "0.0"
p1p2_result_p3: "0"
p1p2p3_16_recovery_amount: "0"
p1p2p3_60_recovery_amount: "0"
p1p2p3_nri_impact_v5_amount: "0"
p2_amount: "0"
p2_recovery_duration: "0.0"
p3_cri_amount: "0"
p3_cri_recovery_duration: "0.0"
p3_nri_amount: "0"
p3_nri_recovery_duration: "0.0"
p3_result_p3: "0"
p4_cri_amount: "1"
p4_cri_second_line_duration: "763.0833333333334"
p4_nri_amount: "1"
p4_nri_impact_v5_amount: "1"
p5_amount: "0"
p5_cri_second_line_duration: "0.0"
parent_location: "华为云"
second_type: "软件问题"
start_time_month: "2021-07-01"
start_time_month_str: "2021-07"
start_time_year: "2021-01-01"
start_time_year_str: "2021"
```

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

1. 更新报表时间，报表名称去掉，权限管理，编辑下拉，登录问题

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


7. 监听路由变化
```ts
import {
  ActivationEnd,
  NavigationStart,
  Router,
  NavigationEnd,
} from "@angular/router";

    this.router.events.subscribe((event: NavigationStart) => {
      if (event instanceof NavigationEnd) {
        // 重新调用接口
    });
```

2021/7/26，还没合并代码

8. 数据加载不出来，有可能是 DOM 已经加载完成，但是数据还没有过来导致的，所以可以加下  *ngIf="show" 来等加载完成之后再展示数据

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

```html
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

15. 1. 柱状图要排序，饼状图要排序，字体搞小一点

## Angular 从入坑到挖坑

16. https://www.cnblogs.com/danvic712/p/getting-started-with-angular-route-guards.html

17 大屏幕一定要提前考虑到小屏幕的适配

18 开发计划

19. angular 父组件如何访问子组件的方法 

https://www.cnblogs.com/starof/p/8689908.html

20 angular 子组件如何访问父组件的方法

https://blog.csdn.net/smallNut/article/details/106048445

21 如何获取并缓存上一级路由，js 缓存

```ts
import { CookieService } from "@src/services/cookie.service";
export class HandleTiTreeSelectService {
  static handleSelectAll(treeNodes) {
    treeNodes.map((item) => {
      item.checked = true;
      if (item.children && item.children.length) {
        item.children.map((item) => {
          item.checked = true;
        });
      }
    });
    return treeNodes;
  }

  static handleSelectValue(treeNodes) {
    return treeNodes.filter((item) => item.checked);
  }

  // 将小数点转成百分比
  static toPercent(value) {
    if (value === null) {
      return (value = 0);
    }
    if (value <= 1 && value > 0) {
      var str = Number((value || 0) * 100).toFixed(2);
      str += "%";
      return str;
    } else {
      return value + "";
    }
  }

  // 设置路由 fullPath
  static setRouteFullPath(routeName) {
    const url = window.location.hash.slice(1);
    CookieService.setItem(routeName, url);
  }

  // 从缓存中获取 fullPath
  static getRouteFullPath(routeName) {
    return CookieService.getItem(routeName);
  }

  // 解析路由参数
  static parseRouteParams(url) {
    let index = url.indexOf("?");
    url = url.substring(index + 1);
    let vars = url.split("&");
    let params = {};
    for (let i = 0; i < vars.length; i++) {
      let pair = vars[i].split("=");
      params[pair[0]] = decodeURIComponent(pair[1]);
    }
    console.log("params===>", params);
    return params;
  }
}
```

22 js unescape string 中文乱码

https://blog.csdn.net/guchuanlong/article/details/8843906

1. 搜索条件筛选，要独立一个组件
2. 麻将块需要抽取一个组件
3. 事件表格


iphonex 如何适配底部小黑条
https://www.jianshu.com/p/cf1909e5e83c


类型分布（已经修复）
事件详情
重点，次重点不对月做筛选（已经修复）
接口全部调用的问题
服务详情下撞有问题（已经修复）

23 对象不要直接赋值，会导致不必要的麻烦

24 detail 页面可以根绝路由参数抽成一个页面

25 echarts 如何格式化数据

https://blog.csdn.net/cysear/article/details/79578174

26 echarts 二次渲染有问题

There is a chart instance already initialized on the dom.

https://blog.csdn.net/u010682330/article/details/78239118

27 解决ECharts Can't get dom width or height!无法初始化图表的问题

https://blog.csdn.net/Axela30W/article/details/77800290

https://blog.csdn.net/wangheng0221/article/details/116313405

28 echarts 如何解决横轴滚动问题

https://blog.csdn.net/qq_27104805/article/details/101084459

https://blog.csdn.net/weixin_48912846/article/details/116086517

29 详情

封装 dialog 组件

https://lq782655835.github.io/blogs/project/vue-dialog-solution.html

30 提示组件

hekigan.github.io/vue-directive-tooltip/

31 多语言

```ts
// dont remove
import Vue from 'vue';
import VueI18n from 'vue-i18n';
import locale from '@aurora/vue-locale';

Vue.use(VueI18n);

function importAll(r) {
  const locale = {};
  r.keys().forEach(key => {
    locale[key.replace('./', '').replace('.json', '')] = r(key);
  });
  return locale;
}

const messages = {
  'en_US': importAll(require.context('./en_US', true, /.json$/)),
  'zh_CN': importAll(require.context('./zh_CN', true, /.json$/))
};

const i18n = locale.initI18n({ VueI18n, messages })

export default i18n;
```

2021-01
[
	[], // P1P2事件 2021-01-2021-12
	[], // P1P2引发P3数量 2021-01-2021-12
	[], // P3NRI事件数量 2021-01-2021-12
	[], // P3NRI事件平均恢复时长 2021-01-2021-12
	[], // 变更导致P3以上NRI事件 2021-01-2021-12
	[], // P1P2平均恢复时长 2021-01-2021-12
	[], // P3引发事件数量 2021-01-2021-12
	[], // P3受损服务恢复时长 2021-01-2021-12
	[], // P4NRI事件数量 2021-01-2021-12
	[], // P4事件数量 2021-01-2021-12
	[], // P4平均二线处理时长 2021-01-2021-12
	[], // P5事件数量 2021-01-2021-12
	[], // 变更引入P4NRI事件数量 2021-01-2021-12
]

[
	[], // p1p2事件 2020-01-2020-12
	[], // P1P2引发P3数量 2020-01-2020-12
	[], // P3NRI事件数量 2020-01-2020-12
	[], // P3NRI事件平均恢复时长 2020-01-2020-12
	[], // 变更导致P3以上NRI事件 2020-01-2020-12
	[], // P1P2平均恢复时长 2020-01-2020-12
	[], // P3引发事件数量 2020-01-2020-12
	[], // P3受损服务恢复时长 2020-01-2020-12
	[], // P4NRI事件数量 2020-01-2020-12
	[], // P4事件数量 2020-01-2020-12
	[], // P4平均二线处理时长 2020-01-2020-12
	[], // P5事件数量 2020-01-2020-12
	[], // 变更引入P4NRI事件数量 2020-01-2020-12
]

 xAxis "1" not found


    <ti-table
      [(displayedData)]="displayed"
      [srcData]="srcData"
      [columns]="columns"
      [searchWords]="searchWords"
      [searchKeys]="searchKeys"
      tiColsResizable
      class="ti3-table-small"
    >
      <div class="ti3-resize-wrapper">
        <div>
          <input
            style="float: right; margin-bottom: 10px"
            tiText
            [(ngModel)]="searchWords[0]"
            placeholder="请输入关键字搜索"
            type="text"
          />
        </div>
        <table>
          <thead>
            <tr>
              <ng-container *ngFor="let column of columns">
                <th
                  tiOverflow
                  *ngIf="column.show || column.show === undefined"
                  width="{{ column.width }}"
                >
                  {{ column.title }}
                </th>
              </ng-container>
            </tr>
          </thead>
          <tbody *ngIf="displayed.length">
            <tr
              *ngFor="let row of displayed; let i = index; trackBy: trackByFn"
            >
              <!-- 事件 -->
              <td
                tiOverflow
                class="color-data"
                *ngIf="tabType === 'events' && row.id"
              >
                <!-- 新加坡 sgp -->
                <a
                  *ngIf="row.id.includes('sgp')"
                  [href]="
                    'https://sgp-inc.rnd.huawei.com/#/incident/treatIncidents?incident_id=' +
                    row.id
                  "
                  target="_blank"
                  >{{ row.id }}</a
                >
                <!-- 国内的 -->
                <a
                  *ngIf="!row.id.includes('sgp')"
                  [href]="
                    'https://inc.rnd.huawei.com/#/incident/treatIncidents?incident_id=' +
                    row.id
                  "
                  target="_blank"
                  >{{ row.id }}</a
                >
              </td>
              <td tiOverflow *ngIf="tabType === 'events'">
                {{ row.incident_id }}
              </td>
              <!-- 问题 -->
              <td tiOverflow *ngIf="tabType === 'problems'" class="color-data">
                <!-- 新加坡 sgp -->
                <a
                  *ngIf="row.incident_id.includes('sgp')"
                  [href]="
                    'https://sgp-inc.rnd.huawei.com/#/issue/treatIssue?issue_id=' +
                    row.incident_id
                  "
                  target="_blank"
                  >{{ row.incident_id }}</a
                >
                <!-- 国内的 -->
                <a
                  *ngIf="!row.incident_id.includes('sgp')"
                  [href]="
                    'https://inc.rnd.huawei.com/#/issue/treatIssue?issue_id=' +
                    row.incident_id
                  "
                  target="_blank"
                  >{{ row.incident_id }}</a
                >
              </td>
              <!-- 变更 -->
              <td tiOverflow *ngIf="tabType === 'changes'" class="color-data">
                <!-- GOMSC && sgp -->
                <a
                  *ngIf="
                    row.incident_id.includes('GOMSC') &&
                    row.incident_id.includes('SGP')
                  "
                  [href]="
                    'https://gocm.sgp.rnd.huawei.com/#/change-approve-process/approve-process?tid=' +
                    row.incident_id
                  "
                  target="_blank"
                  >{{ row.incident_id }}</a
                >
                <!-- GOMSC && !sgp -->
                <a
                  *ngIf="
                    row.incident_id.includes('GOMSC') &&
                    !row.incident_id.includes('SGP')
                  "
                  [href]="
                    'https://gocm.rnd.huawei.com/#/change-approve-process/approve-process?tid=' +
                    row.incident_id
                  "
                  target="_blank"
                  >{{ row.incident_id }}</a
                >
                <!-- !GOMSC && sgp -->
                <a
                  *ngIf="
                    !row.incident_id.includes('GOMSC') &&
                    row.incident_id.includes('SGP')
                  "
                  [href]="
                    'http://sgp-ccc.rnd.huawei.com/#/ccc/process/apply?tid=' +
                    row.incident_id
                  "
                  target="_blank"
                  >{{ row.incident_id }}</a
                >

                <!-- !GOMSC && !sgp -->
                <a
                  *ngIf="
                    !row.incident_id.includes('GOMSC') &&
                    !row.incident_id.includes('SGP')
                  "
                  [href]="
                    'http://ccc.rnd.huawei.com/#/ccc/process/apply?tid=' +
                    row.incident_id
                  "
                  target="_blank"
                  >{{ row.incident_id }}</a
                >
              </td>
              <td tiOverflow>
                {{ row.created_time }}
              </td>
              <td tiOverflow>
                {{ row.status }}
              </td>
              <td tiOverflow>
                {{ row.level_name }}
              </td>
              <td tiOverflow>
                {{ row.department_name }}
              </td>
              <td tiOverflow>
                {{ row.scope_name }}
              </td>
              <td tiOverflow>
                {{ row.service_name }}
              </td>
              <td tiOverflow>
                {{ row.first_type }}
              </td>
              <td tiOverflow>
                {{ row.second_type }}
              </td>
              <td tiOverflow>
                {{ row.region }}
              </td>
              <td tiOverflow>
                {{ row.location }}
              </td>
            </tr>
          </tbody>
          <tbody *ngIf="displayed.length === 0">
            <tr align="center" class="ti3-table-nodata">
              <td tiColspan>暂无表格数据</td>
            </tr>
          </tbody>
        </table>
      </div>
      <ti-pagination
        [(currentPage)]="pagination.currentPage"
        [pageSize]="pagination.pageSize"
        [(totalNumber)]="pagination.totalNumber"
        style="padding: 0 0 10px 10px"
      >
      </ti-pagination>
    </ti-table>



3. 混合云默认不筛选
4. 运营部默认不筛选

麻将块，部门排行， 

(筛选年，筛选部门，没有区域)
SLO不要动

跳转到领域，服务不传  is_cloud_bu

1. 趋势图的 y 轴不要
2. 给表格添加点击事件

注意

1. 接口是哪个先调用，后调用需要注意
2. 优化的点，只加载第一个 tab


promise.all() 如何先请求一个接口，再请求其他接口

```ts
     Promise.all([this.initDepartName(), this.initServiceName()]).then((res) => {
      console.log("res===>", res);
    });
  // 初始化部门接口
  async initDepartName() {
    return new Promise(() => {
      this.chartsService
        .postDimensionTree({ ...this.tiSelectDepartParams })
        .toPromise()
        .then((res) => {
          this.tiSelectDepart.options = res.data;
          this.tiSelectDepart.value = res.data.map((item) => item);
        });
    });
  }

  // 初始化服务接口
  async initServiceName() {
    return new Promise(() => {
      this.chartsService
        .postDimensionTree({ ...this.tiSelectServiceParams })
        .toPromise()
        .then((res) => {
          this.tiSelectService.options = res.data;
          this.tiSelectService.value = res.data.map((item) => item);
        });
    });
  }
```
1. 什么时候给， deadline

2. 导出功能可以封装成一个全局的方法

3. 刷新筛选器

4. 可以通过判断链接是否包含 http:// 如果不包含，我们就直接不展示 iframe 直接 跳转

5. angular使用@viewChild父组件获取子组件的数据和方法

6 修改了 左侧菜单组件

7 navigateByUrl vs navigate

https://github.com/deepthan/blog-angular/issues/54

8 深耕 angular 

https://github.com/deepthan/blog-angular


https://stackoverflow.com/questions/46990389/angular-4-no-component-factory-found-did-you-add-it-to-ngmodule-entrycomponent


[
	[局点，未闭环，已闭环],
	[中国电信，2，0],
	[混彭云，2，3]
]


安卓手机你拿不到 cookie 改用 localStorage
 

Echarts图例位置 - legend属性

https://blog.csdn.net/woteafuck/article/details/91986313


1. https://stackoverflow.com/questions/43937387/if-selector-is-an-angular-component-then-verify-that-it-is-part-of-this-mod
2. https://stackoverflow.com/questions/39428132/custom-elements-schema-added-to-ngmodule-schemas-still-showing-error


ng g c 
ng g module 
ng g service 


环比有问题



1、页面加载问题

服务变更
变更详细数据导出

监控告警SLO初始化月年有问题 done，图表只传more done

重犯问题，共性问题同比环比 done

平台 ONCALL 搞个链接

调整事件麻将块的位置


研究 js 前端缓存问题
研究 localStorage, cookie, sessionStorage
研究 JSON.parse, JSON.stringify 

对象里面不要写方法，有可能会用到 JSON.parse, JSON.stringify 


  // 将函数方法转成字符串
  static funToStr(fn) {
    return fn.toString();
  }

  // 将函数方法字符串转成函数
  static fnStrToFun(fnStr, self) {
    return new Function(`return ${fnStr}`).bind(self);
  }

  // 将对象转成字符串
  static objToStr(obj) {
    for (let key in obj) {
      if (typeof obj[key] === "function") {
        obj[key] = HandleTiTreeSelectService.funToStr(obj[key]);
      }
    }

    return JSON.stringify(obj);
  }

  // 将字符串对象转成对象
  static strObjToObj(strObj, self) {
    const obj = JSON.parse(strObj);
    console.log("obj[key]===>", obj);
    for (let key in obj) {
      let value = obj[key];
      console.log("obj[key]===>", obj[key], obj);
      if (typeof value === "string" && value.indexOf("=>") > -1) {
        obj[key] = HandleTiTreeSelectService.fnStrToFun(obj[key], self)();
      }
    }
    return obj;
  }



评估一个工作量

当个数据饼状图颜色有问题

爆粗问题如何一步步推导

代码逻辑，哪个先执行，哪个后执行，代码执行顺序出现了问题是最难找的

发布订阅

SLO概览
本年度红灯服务
月度红灯服务
云服务事件列表

Angular 父组件通过@ViewChild 主动获取子组件的数据和方法
https://blog.csdn.net/I_r_o_n_M_a_n/article/details/114988365?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.tagcolumn&spm=1001.2101.3001.4242.0



import {Injectable} from '@angular/core';
import {Subject, Observable} from 'rxjs';

// 发布订阅，用于更新头部菜单
@Injectable({
  providedIn: 'root'
})
export class MenuService {
  private subject = new Subject();

  send() {
    this.subject.next();
  }

  get(): Observable<any> {
    return this.subject.asObservable();
  }
}


detail接口:
    问题重犯:新增重犯问题数,新增重犯问题导致P3+事件数,新增重犯问题导致P4+事件数
    共性问题:新增共性风险数,新增致命严重共性风险数,累计共性风险未闭环数,累计致命严重共性风险未闭环数


1、点击时间筛选（上月，本月，去年，本年来回切换）的时候，加载圈有时候有有时候没有
2、点击详情的时候，直接显示暂无数据（没有加载圈），3s之后显示数据。
3、事件，变更同时加载，到之后数据加载奇慢无比
4、在上月，本月来回切换的时候，加载有时候很慢（超过3s）,有时候又很快。看看是否是缓存的原因


新总览存在的问题
	3,事件下钻明细缺少中断时长字段 -- 张浩中/李金文
	4,前端分页签请求加载数据,事件变更可以一起加载,其他页签点哪个页签加载哪个页签的数据 --	李金文
	5,数据加载的时候有时候转圈有时候不转圈,统一改成转圈的 -- 李金文
	5,下钻后记录返回页面参数回填 -- 解决时间待定,李金文
	6,详情页加载速度优化 --张浩中


1. 先看看有没有UT，不过HW的尿性 一般有也起不到防护作用
2. 如果没有，你看看能不能加UT，不过HW的尿性，一般也不好加
3. 如果方法太长，抽取方法，不改逻辑，这个一般比较保险，直到抽到你要改的代码函数比较小，也就好改好测试了


问题月度统计

Wed Sep 01 2021 00:00:00 GMT+0800

1. 解决日期的拷贝 转成时间戳
2. 解决筛选器，需要将缓存的数据跟原始数据匹配


日期

https://www.cnblogs.com/polk6/p/4156595.html

好叼哦

https://www.cnblogs.com/polk6/archive/2013/05/10/3071327.html

git 版本回退

https://blog.csdn.net/yxlshk/article/details/79944535

1. 登录提示无权限问题 已解决
2. 区域筛选,混合云和行业云默认不选 已解决
3. 返回到总览有BUG 变更 已解决
4. 区域筛选,混合云和行业云默认不选 已解决
5. 详情下钻返回的时候保留筛选器内容 已解决
6. 点击链接加上下划线 已解决
7. 顶部链接导航 hover 变色 已解决
8. 详情下转字段不对 full_service_name 已解决

js 如何判断 pc 端 和 移动端

https://blog.51cto.com/aeolian/2853151

如何监听路由变化

https://blog.csdn.net/wjyyhhxit/article/details/106673196

Angular 学习笔记82：监听路由的变化

页面很多请求需要主要loading 效果的处理

1. promise.all

https://blog.csdn.net/wjyyhhxit/article/details/106673196
月度，部门，麻将块，同比，环比 已解决

第一个问题 问题页面服务下转返回参数没了 已解决
第二个问题 变更问题，服务层级刷选时间不生效 已解决
第三个问题 变更新加的两行要加下转 已解决
第四个问题 问题累计未闭环数和累计超期未闭环数时间只传less不传more 已解决


事件：2021发生P1P2NRI事件数量<span style='font-weight: bold'>4</span>起，较上年<span style='color: rgb(80,212,171);font-weight: bold'>减少</span><span style='font-weight: bold'>8</span>起,P1P2 NRI平均恢复时长<span style='font-weight: bold'>39</span>min，较上年<span style='color: rgb(80,212,171);font-weight: bold'>减少</span><span style='font-weight: bold'>101</span>min</br>&emsp;&emsp;&emsp;发生P3 NRI事件<span style='font-weight: bold'>57</span>起，较上年<span style='color: rgb(80,212,171);font-weight: bold'>减少</span><span style='font-weight: bold'>88</span>起，P3NRI平均恢复时长<span style='font-weight: bold'>167</span>min,较上年<span style='color: rgb(246,111,106);font-weight: bold'>增加</span><span style='font-weight: bold'>98.8</span>min</br>&emsp;&emsp;&emsp;P1P2 NRI事件:产品/安全问题<span style='font-weight: bold'>1</span>起;供应商问题/运营商故障<span style='font-weight: bold'>0</span>起</br>&emsp;&emsp;&emsp;P3 NRI事件:产品问题<span style='font-weight: bold'>24</span>起(含安全问题<span style='font-weight: bold'>0</span>起,性能问题<span style='font-weight: bold'>7</span>)起,变更问题<span style='font-weight: bold'>7</span>起<br/><br/>变更：2021变更总量<span style='font-weight: bold'>901100</span>次，较上年<span style='color: rgb(80,212,171);font-weight: bold'>增加</span><span style='font-weight: bold'>476434</span>次;变更成功率<span style='font-weight: bold'>98.69%</span>，较上年<span style='color: rgb(80,212,171);font-weight: bold'>增加</span><span style='font-weight: bold'>1.28%</span>；自动化率<span style='font-weight: bold'>54.73%</span>，较上年<span style='color: rgb(80,212,171);font-weight: bold'>增加</span><span style='font-weight: bold'>5.1%</span>；工具化率<span style='font-weight: bold'>96.29%</span>，较上年<span style='color: rgb(80,212,171);font-weight: bold'>增加</span><span style='font-weight: bold'>9.19%</span></br>&emsp;&emsp;&emsp;变更导致P3及以上事件<span style='font-weight: bold'>10</span>起，较上年<span style='color: rgb(80,212,171);font-weight: bold'>减少</span><span style='font-weight: bold'>26</span>起<br/><br/>问题：2021致命&严重问题数量<span style='font-weight: bold'>844</span>个，较上年<span style='color: rgb(80,212,171);font-weight: bold'>减少</span><span style='font-weight: bold'>1314</span>个；现网致命&严重问题及时解决率<span style='font-weight: bold'>83.33%</span>，较上年<span style='color: rgb(80,212,171);font-weight: bold'>增加</span><span style='font-weight: bold'>12.68%</span>


总览还有一个没做，访问打点

https://github.com/AngularID-CN/AngularID-CN/tree/master/articles

webcomponent

https://github.com/mdn/web-components-examples/


路由变化，怎么加载对应页面


vue 跳转同一路由,页面不刷新解决方案

https://blog.csdn.net/qq_35897953/article/details/107906851

https://zhuanlan.zhihu.com/p/365746526

git rebase -i head~11


git 

解决关于 npm build --prod ，出现 ERROR in budgets, maximum exceeded for initial. Budget 5 MB was exceeded by 750 kB的问题
https://www.cnblogs.com/huangenai/p/11865328.html


git add .
git commit -m"RR2021050600855 "
git push

1,多选埋点优化 已解决
2,菜单管理操作,阻塞后再返回 已解决
3,偶现月不能点击,年可以点击的BUG
4,左侧菜单消失的问题

点了左侧菜单的总览，这个时候刚好过期

git commit 

git checkout zhz/java_1102

git pull

git merge

git push


git rebase 冲突

剞劂

sre_bi.

<app-chart v-click="click" />

click() {
	overview.emit('') this.$emit('overview', '')
}

y00314601

回填报错有可能是接口数据请求还没回来

1. 权限控制需要注意：用户自己输入，需要进行路由拦截

https://stackoverflow.com/questions/55925936/angular-7-throttling-navigation-to-prevent-the-browser-from-hanging


Invalid configuration of route '/': path cannot start with a slash


https://stackoverflow.com/questions/38596016/angular-2-router-error-invalid-configuration-of-route-undefined/51039904

  {
    path: "",
    redirectTo: "/charts/home",
    pathMatch: "full",
  },

ServiceReepairOverview

  {
    path: "faults",
    loadChildren: () =>
      import("./faults/faults.module").then((mod) => mod.FaultsModule),
  },

/whitelistconfig/home?isFullScreen=true

echarts 图有又边距 right: 0
