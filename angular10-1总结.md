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

```ts
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
```
 xAxis "1" not found


3. 混合云默认不筛选
4. 运营部默认不筛选

麻将块，部门排行， 

(筛选年，筛选部门，没有区域)
SLO不要动

### 跳转到领域，服务不传  is_cloud_bu

1. 趋势图的 y 轴不要
2. 给表格添加点击事件

### 注意

1. 接口是哪个先调用，后调用需要注意
2. 优化的点，只加载第一个 tab


### promise.all() 如何先请求一个接口，再请求其他接口

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
