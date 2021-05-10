[badge](https://element.eleme.cn/#/zh-CN/component/badge)

- value 展示的值
- max 设置最大值
- is-dot 是否展示原点
- hidden 隐藏
- type 类型

type 类型需要验证

- 'primary', 
- 'success', 
- 'warning', 
- 'info', 
- 'danger'

```js
type: {
  type: String,
  validator(val) {
    return ['primary', 'success', 'warning', 'info', 'danger'].indexOf(val) > -1;
  }
}
```

展示内容的计算属性

```js
computed: {
  content() {
    if (this.isDot) return;

    const value = this.value;
    const max = this.max;

    if (typeof value === 'number' && typeof max === 'number') {
      return max < value ? `${max}+` : value;
    }

    return value;
  }
}
```
