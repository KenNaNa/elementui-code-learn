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
  })
  console.log('url====>', url)
  // first update color
  // TIPS: THEME COLOR HANDLER!! PLEASE CHECK THAT!!
  if (process.env.NODE_ENV !== 'production' || process.env.VUE_APP_PREVIEW === 'true') {
    updateTheme(this.settings.primaryColor)
  }
},
```


