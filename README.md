# 电商项目的总结

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


