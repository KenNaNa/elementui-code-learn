头像组件使用 render jsx 的方式渲染的

- 图标大小
- 头像形状
- 头像链接
- fit
- srcSet


size 属性验证了三种属性

- large
- medium
- small

```js
size: {
  type: [Number, String],
  validator(val) {
    if (typeof val === 'string') {
      return ['large', 'medium', 'small'].includes(val);
    }
    return typeof val === 'number';
  }
},
```

shape 属性验证三种状态

- circle
- square

```js
shape: {
  type: String,
  default: 'circle',
  validator(val) {
    return ['circle', 'square'].includes(val);
  }
},
```
