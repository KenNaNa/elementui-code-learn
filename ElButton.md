[button](https://element.eleme.cn/#/zh-CN/component/button)

- size 大小
- type 类型
- plain 是否朴素按钮	
- round 是否圆角按钮	
- circle 是否圆形按钮	
- loading 是否加载中状态	
- disabled 是否禁用状态	
- icon 图标类名	
- autofocus 是否默认聚焦	
- native-type	原生 type 属性	

```html
<template>
  <button
    class="el-button"
    @click="handleClick"
    :disabled="buttonDisabled || loading"
    :autofocus="autofocus"
    :type="nativeType"
    :class="[
      type ? 'el-button--' + type : '',
      buttonSize ? 'el-button--' + buttonSize : '',
      {
        'is-disabled': buttonDisabled, // 禁止的类名
        'is-loading': loading,
        'is-plain': plain,
        'is-round': round,
        'is-circle': circle
      }
    ]"
  >
    <i class="el-icon-loading" v-if="loading"></i>
    <i :class="icon" v-if="icon && !loading"></i>
    <span v-if="$slots.default"><slot></slot></span>
  </button>
</template>
```

