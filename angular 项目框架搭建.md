1. 项目公共样式，配置

2. 项目图标，配置

3. 项目多语言，配置

4. 项目路径重定向，配置

```js
 resolve: {
    extensions: ['', '.js', '.vue'],
    fallback: [path.join(__dirname, '../node_modules')],
    alias: {
      'vue$': 'vue/dist/vue',
      'src': path.resolve(__dirname, '../src'),
      'assets': path.resolve(__dirname, '../src/assets'),
      'components': path.resolve(__dirname, '../src/components'),
      "jquery": path.resolve(__dirname, '../node_modules/jquery/dist/jquery.slim.min.js'),
      "bootstrap": path.resolve(__dirname, '../src/plugin/bootstrap'),
    }
  }
```

5. 项目组件要不要全局注册

6. 项目 api url 统一配置

7. 
