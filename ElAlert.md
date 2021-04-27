为什么从 alert 组件开始呢？因为这个组件相对简单，容易入门，又可以从中窥探 element-ui 组件中一些设计规则

```js
packages
 - alert
   - main.vue
 - index.js
```

# 类型 type

从源码的研究我们可以看到 alert 有四种类型

- info 默认类型
- success 成功
- warning 告警
- error 错误

所以组件 props 里面有 type 字段

# 内容，标题，描述

一般像这种属于提示组件都会有标题，描述，所以组件里面有

- title 标题
- description 描述

# 是否可以关闭

- closable 默认为 true

# 可以展示自定义的图标按钮

- showIcon

# 内容是否呈现于中间

- center

# effect 效果

- effect

# 计算属性
我们注意到源码中有好几个计算属性

```js
const TYPE_CLASSES_MAP = {
  'success': 'el-icon-success',
  'warning': 'el-icon-warning',
  'error': 'el-icon-error'
};
computed: {
  // 类型
  typeClass() {
    return `el-alert--${ this.type }`;
  },
  // icon 
  iconClass() {
    return TYPE_CLASSES_MAP[this.type] || 'el-icon-info';
  },
  // 内容加大
  isBigIcon() {
    return this.description || this.$slots.default ? 'is-big' : '';
  },
  // 内容加粗
  isBoldTitle() {
    return this.description || this.$slots.default ? 'is-bold' : '';
  }
}
```

`this.$slots.default` 用于解析默认插槽的内容
