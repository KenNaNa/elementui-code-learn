# 电商项目的总结

给你七年时间，你会干什么？我想不出我会干什么耶，我只有确定的一件事情就是我会全国各地跑一遍

有人开源躺平

[开源躺平](https://github.com/phodal/tangping)

1. 登录跳转问题，不需要 vuex 存储状态了，因为刷新 vuex 数据丧失了，需要使用缓存机制
2. tabbar 页面与非tabbar页跳转互动问题
3. 订单页面，需要提示用户去登录的，
4. 商品列表，单个列表项的删除功能，添加，减少，清空操作，用户操作，用户体验
5. 商品规格不应该弄一个管理，而是应该让用户在添加商品的过程自己去添加
6. 多测试

- 登录或许可以写成一个公用的组件

- 删除一些无用的代码

- 重复点击菜单栏项，需要刷新页面
- 资源共享的功能
- 打印机打印的问题
- 拖拽插件
- 添加小标签问题
- 小类，大类一般是怎么添加的
- 研究三级联动组件
- 环境配置问题，提取公共的 url
- 上传组件需要添加loading状态
- 图片需要懒加载
- [vue中的图片加载与显示默认图片](https://www.cnblogs.com/xulei1992/p/6558294.html)
- [vue项目设置img标签的默认图片](https://blog.csdn.net/xjun0812/article/details/104414838)
- [基于阿里Ant Design of Vue的上传组件二次封装](https://blog.csdn.net/weixin_43056561/article/details/107834801)
- 301 logout 重定向到登录页面
- 提取 store 几个组件到一个 home/store 目录，方便管理
- 402 非法类别
- 写着，写着麻烦，要停下来想想有没有更好的方式
- [基于阿里Ant Design of Vue的上传组件二次封装](https://blog.csdn.net/weixin_43056561/article/details/107834801?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_v2~rank_aggregation-1-107834801.pc_agg_rank_aggregation&utm_term=ant+design+vue+%E4%B8%8A%E4%BC%A0%E7%BB%84%E4%BB%B6%E5%B0%81%E8%A3%85&spm=1000.2123.3001.4430)
- [Cookie、 session、localStorage 和sessionStorage的存储区别](https://blog.csdn.net/V8min/article/details/115794263?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_v2~rank_aggregation-8-115794263.pc_agg_rank_aggregation&utm_term=%E6%B5%8F%E8%A7%88%E5%99%A8%E5%85%B3%E9%97%AD%E4%B9%8B%E5%90%8Ecookie%E6%B6%88%E5%A4%B1&spm=1000.2123.3001.4430)
- 添加店铺描述
- 为什么要这么抽取，为什么要这么实现
- 像产品思维那样转变
- 领导是结果导向的，员工是过程导向的，我们应该像结果导向过渡，那么要怎么做
- 为什么要抽丝剥茧，做到这一步的理由是什么，为什么要这么做
- 提取公共的样式
- 添加按钮组件修改宽高度文图
- 提取公共的颜色，字体大小变量
- 提取 js 中的常量，比如数字
- 直接定义变量直接可以全局使用
- [在vue中自动生成文件以及自动引入component，router、vuex按模块划分](https://blog.csdn.net/qq_42268364/article/details/106905870?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_v2~rank_aggregation-5-106905870.pc_agg_rank_aggregation&utm_term=vue%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E6%96%87%E4%BB%B6&spm=1000.2123.3001.4430)
- [瀑布流组件](https://github.com/AwesomeDevin/vue-waterfall2)
- 将 StoreModal, StoreCard 抽取到 Store 目录
- 商品模块使用卡片式模块，方便定位
- 可增加，删除，编辑的树组件，数据在组件里面处理，只需要将处理之后的数据暴露出来就行了
- 使用级联的方式去搜索对应的产品 级联的数据应该在里面处理，只需要将处理之后的数据暴露出来就行了
- 商品列表组件，只需要将处理之后的数据暴露出来就行了
- 滚动定位，https://www.cnblogs.com/haonanZhang/p/9517636.html
- [低代码快速搭建完整商品列表页](https://blog.csdn.net/weixin_47095348/article/details/110670641)
- [que-etc/resize-observer-polyfill](https://github.com/que-etc/resize-observer-polyfill)
- [前端虚拟列表的实现原理](https://mp.weixin.qq.com/s/IC5yNkaOfvM9M-s3tj-jsg)
- [ant-design-vue锚点](https://blog.csdn.net/weixin_47711357/article/details/114914953) 
- [多个数组笛卡尔积-js算法](https://blog.csdn.net/suprezheng/article/details/86558050)
- [js 生成笛卡尔积](https://www.cnblogs.com/majiang/p/5988093.html)
- [商品规格SKU笛卡尔积计算](https://blog.csdn.net/lihefei_coder/article/details/106668953)
- [Erupt Framework   🚀   通用后台管理框架](https://github.com/erupts/erupt-web)
- [国内低代码平台从业者交流](https://github.com/taowen/awesome-lowcode)
- [低代码平台, 可视化编辑器，单手打代码，解放你的双手](https://github.com/LianjiaTech/hetu)
- [登录详解(VUE前端) · 低代码开发平台文档 · 看云](https://www.kancloud.cn/frank258jun/sca_manual/2242081)
- [微前端项目实战](https://github.com/hql7/wl-micro-frontends)
- [Angular-HMR](https://github.com/GoogleSmileMMa/Angular-HMR)

- 今天的目标是把查询页面搞出来，规格添加页面
- 今天对接规格创建，添加，修改
- 今晚把基本商品添加页，图片上传基本信息，搞完
- 今晚处理，单规格，多规格问题
- 多规格表格上传，删除处理要回现处理规格，单规格(6-8)
- 分类也要点击弹框，可以修改添加，编辑
- 树组件有个隐藏的风险，就是可能误添加上下级关系，给出添加，修改各个提示


三种价格设置

```html
<template>
  <div class="test">
    <a-radio-group name="radioGroup" :default-value="1" v-model="selected">
      <a-radio :value="1"> 零售价 </a-radio>
      <a-radio :value="2"> 批发价 </a-radio>
      <a-radio :value="3"> 团购价 </a-radio>
    </a-radio-group>
    <a-form-model ref="ruleForm" :model="ruleForm" layout="inline">
      <div v-show="selected === 1">
        <a-form-model-item label="销售价" prop="price">
          <a-input placeholder="请输入销售价" />
        </a-form-model-item>
        <a-form-model-item label="吊牌价" prop="priceTag">
          <a-input placeholder="请输入吊牌价" />
        </a-form-model-item>
        <a-form-model-item label="是否设置为默认价格">
          <input type="radio" name="radio" :value="value" />
        </a-form-model-item>
      </div>
      <div v-show="selected === 2">
        <a-form-model-item label="销售价" prop="price">
          <a-input placeholder="请输入销售价" />
        </a-form-model-item>
        <a-form-model-item label="吊牌价" prop="priceTag">
          <a-input placeholder="请输入吊牌价" />
        </a-form-model-item>
        <a-form-model-item label="是否设置为默认价格">
          <input type="radio" name="radio" :value="value" />
        </a-form-model-item>
      </div>
      <div v-show="selected === 3">
        <a-form-model-item label="销售价" prop="price">
          <a-input placeholder="请输入销售价" />
        </a-form-model-item>
        <a-form-model-item label="吊牌价" prop="priceTag">
          <a-input placeholder="请输入吊牌价" />
        </a-form-model-item>
        <a-form-model-item label="是否设置为默认价格">
          <input type="radio" name="radio" :value="value" />
        </a-form-model-item>
      </div>
    </a-form-model>
  </div>
</template>

<script>
export default {
  name: "test",
  data() {
    return {
      ruleForm: {},
      selected: 1,
      labelCol: { span: 4 },
      wrapperCol: { span: 14 },
      value: true,
    };
  },
};
</script>
```


```js
function cartesianProductOf(...args) {
  return args.reduce(
    (total, current) => {
      let ret = [];
      total.forEach(a => {
        current.forEach(b => {
          ret.push(a.concat([b]));
        });
      });
      return ret;
    },
    [
      []
    ]
  );
}
```

```js
// main.js
const components = require('./components/index')
for (let componentName in components) {
  Vue.component(componentName, components[componentName])
}
```

```js
// vue.config.js
const path = require("path");

function resolve(dir) {
  return path.join(__dirname, dir);
}

module.exports = {
  chainWebpack: config => {
    config.resolve.alias
      .set('components', resolve('src/components/index.js'))
  },
}
```

```js
// index.js
const componentFiles = require.context('./', true, /index.js$/)
const components = componentFiles.keys().reduce((files, filePath) => {
  const fileName = filePath.replace(/^\.\/(.*)\/index\.\w+$/, '$1')
  const value = componentFiles(filePath)
  if (value.default) {
    const componentName = fileName.split('/')[0]
    files[componentName] = value.default
  } else {
    for (let key in value) {
      console.log("key===>", key, value[key])
      files[key] = value[key]
    }
  }
  return files
}, {})
module.exports = components
```


```js
// entryTemplate.js
module.exports = {
  entryTemplate: (compoenntName) => {
    return `
      import ${compoenntName} from './src'
      export default ${compoenntName}
    `
  }
}
```

```js
// genVueTpl.js
// index.js
const chalk = require('chalk')
const path = require('path')
const fs = require('fs')
const resolve = (...file) => path.resolve(__dirname, ...file)
const log = message => console.log(chalk.green(`${message}`))
const successLog = message => console.log(chalk.blue(`${message}`))
const errorLog = error => console.log(chalk.red(`${error}`))
// 导入模板
const {
  vueTemplate
  // entryTemplate
} = require('./template')
// 导入入口
const {
  entryTemplate
} = require('./entryTemplate')
// 生成文件
const generateFile = (path, data) => {
  if (fs.existsSync(path)) {
    errorLog(`${path}文件已存在`)
    return
  }
  return new Promise((resolve, reject) => {
    fs.writeFile(path, data, 'utf8', err => {
      if (err) {
        errorLog(err.message)
        reject(err)
      } else {
        resolve(true)
      }
    })
  })
}
log('请输入要生成的vue文件夹名称 views: xxx、comp: xxx、pageComp: xxx、 它们会生成在对应的文件目录下')
let componentName = ''
process.stdin.on('data', async chunk => {
  // 组件名称
  const inputName = String(chunk).trim().toString().split(':')[1]
  // 判断放在那个文件夹里面
  let pathName = String(chunk).trim().toString().split(':')[0]

  let componentPath = null
  let entryFile = null
  switch (pathName) {
    case 'views':
      pathName = 'views'
      componentPath = resolve(`../src/${pathName}`, inputName)
      break
    case 'comp':
      pathName = 'components'
      componentPath = resolve(`../src/${pathName}`, inputName, 'src')
      entryFile = resolve(`../src/${pathName}`, inputName, 'index.js')
      break
    case 'pageComp':
      pathName = 'pageComponents'
      componentPath = resolve(`../src/${pathName}`, inputName, 'src')
      entryFile = resolve(`../src/${pathName}`, inputName, 'index.js')
      break
  }
  // Vue页面组件路径

  // vue文件
  const vueFile = resolve(componentPath, 'index.vue')

  // 入口文件

  // 判断组件文件夹是否存在
  const hasComponentExists = fs.existsSync(componentPath)
  if (hasComponentExists) {
    errorLog(`${inputName}页面组件已存在，请重新输入`)
    return
  } else {
    log(`正在生成 ${inputName} 的目录 ${componentPath}`)
    await dotExistDirectoryCreate(componentPath)
    if (pathName === 'views') {
      log(`正在生成页面子组件 components 的目录 ${componentPath}\\components`)
      await fs.mkdir(`${componentPath}\\components`, err => {
        log(err)
      })
    }
  }
  try {
    // 获取组件名
    if (inputName.includes('/')) {
      const inputArr = inputName.split('/')
      componentName = inputArr[inputArr.length - 1]
    } else {
      componentName = inputName
    }
    log(`正在生成 vue 文件 ${vueFile}`)
    await generateFile(vueFile, vueTemplate(componentName))
    log(`正在生成 entry 文件 ${entryFile}`)
    if (entryFile) {
      await generateFile(entryFile, entryTemplate(componentName))
    }
    successLog('生成成功')
  } catch (e) {
    errorLog(e.message)
  }

  process.stdin.emit('end')
})
process.stdin.on('end', () => {
  log('exit')
  process.exit()
})

function dotExistDirectoryCreate(directory) {
  return new Promise((resolve) => {
    mkdirs(directory, function () {
      resolve(true)
    })
  })
}
// 递归创建目录
function mkdirs(directory, callback) {
  var exists = fs.existsSync(directory)
  if (exists) {
    callback()
  } else {
    mkdirs(path.dirname(directory), function () {
      fs.mkdirSync(directory)
      callback()
    })
  }
}
```

```js
// template.js
module.exports = {
  vueTemplate: compoenntName => {
    return `<template>
  <div class="${compoenntName}__wrapper"></div>
</template>

<script>
export default {
  name: '${compoenntName}',

  components: {},

  mixins: [],

  props: {},

  data() {
    return {}
  },

  computed: {},

  watch: {},

  created() {},

  mounted() {},

  destroyed() {},

  methods: {}
}
</script>

<style lang="scss" scoped>
  .${compoenntName}__wrapper {

  }
</style>
`
  }
}
```

```js
/*第一层if判断生产环境和开发环境*/
if (process.env.NODE_ENV === 'production') {
    /*第二层if，根据.env文件中的VUE_APP_FLAG判断是生产环境还是测试环境*/
    if (process.env.VUE_APP_FLAG === 'pro') {
        //production 生产环境
        axios.defaults.baseURL = 'http://api.xinggeyun.com';//路径

    } else {
        //test 测试环境
        axios.defaults.baseURL = 'http://192.168.0.152:8102';//路径
   }
} else { //dev 开发环境 
   axios.defaults.baseURL = 'http://192.168.0.152:8102';//路径
}
```

```html
 <a-form-item label='地址' :colon="false">
  <a-cascader
    :allowClear="false"
    v-decorator="['area',{rules: [{ required: true, message: '请选择地址' }]}]"
    :options="areaList"
    placeholder="请选择地址"
    :loadData="loadAreaData"
    @change="onAreaChange"
    :getPopupContainer="(trigger) => {return trigger.parentElement}"
  ></a-cascader>
</a-form-item>
```

```js
data () {
    return {
      areaList: [], // 地区数据
    }
  },
  async mounted () {
   // 获取省数据
    this.areaList = await this.getAreaList() || []
  },
methods: {
 /**
     * 获取区域
     */
    getAreaList (code) {
      return new Promise((resolve, reject) => {
        getAreaData({
          code: code ? String(code) : ''
        }).then(res => {
          console.log('获取区域------', res)

          if (res.code === '0') {
            let arr = res.data.map(item => {
              return {
                value: item.code + '',
                label: item.name,
                isLeaf: item.level === '3'
              }
            })
            return resolve(arr)
          } else {
            return resolve([])
          }
        }).catch((err) => {
          return reject(err)
        })
      })
    },
// 获取下一级数据
   async loadAreaData (selectedOptions) {
      if (!this.areaList.length) {
        this.areaList = await this.getAreaList() || []
      } else {
        const targetOption = selectedOptions[selectedOptions.length - 1]
        targetOption.loading = true
        let children = await this.getAreaList(targetOption.value) || []
        if (children.length) {
          targetOption.loading = false
          targetOption.children = children
        } else {
          targetOption.loading = false
          targetOption.isLeaf = true
        }
      }
      this.areaList = cloneDeep(this.areaList)
    },
  // 选择区后 
    onAreaChange (val, selectedOptions) {
      this.provinceCode = selectedOptions[0] ? selectedOptions[0].value : ''
      this.province = selectedOptions[0] ? selectedOptions[0].label : ''
      this.cityCode = selectedOptions[1] ? selectedOptions[1].value : ''
      this.city = selectedOptions[1] ? selectedOptions[1].label : ''
      this.regionCode = selectedOptions[2] ? selectedOptions[2].value : ''
      this.region = selectedOptions[2] ? selectedOptions[2].label : ''
    },
}
```
- 研究权限问题

```js
mounted () {
  const userAgent = navigator.userAgent
  if (userAgent.indexOf('Edge') > -1) {
    this.$nextTick(() => {
      this.collapsed = !this.collapsed
      setTimeout(() => {
        this.collapsed = !this.collapsed
      }, 16)
    })
  }
  const oMenus = document.querySelector('.ant-menu.ant-menu-inline.ant-menu-root.ant-menu-dark')
  oMenus.addEventListener('click', (e) => {
    const url = e.target.getAttribute('href')
    if (url) {
      this.$router.push({
        path: url + '?t=' + new Date().getTime()
      })
    }
    setTimeout(() => {
      this.$router.push(url)
    }, 16)
    console.log('url====>', url)
  })
  // first update color
  // TIPS: THEME COLOR HANDLER!! PLEASE CHECK THAT!!
  if (process.env.NODE_ENV !== 'production' || process.env.VUE_APP_PREVIEW === 'true') {
    updateTheme(this.settings.primaryColor)
  }
},
```

- 在 router-view 加上 key

```html
<router-view :key="getPath"/>
```

- 在computed

```js
computed: {
  ...mapState({
    // 动态主路由
    mainMenu: state => state.permission.addRouters
  }),
  getPath () {
    return this.$route.fullPath
  }
},
```


