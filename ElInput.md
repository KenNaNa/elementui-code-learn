今天进入 input 组件的研究

[input](https://element.eleme.cn/#/zh-CN/component/input)

看了官网之后我们来看看 input 到底有多少种用法

- 普通的用法 v-model
- 禁用状态 disabled
- 可清空 clearable
- 密码框
- 带 icon 的输入框
- 文本域 type
- 可自适应文本高度的文本域 resize
- 复合型输入框 有前置，后置内容
- 尺寸 size
- 带输入建议
- 自定义模板
- 远程搜索
- 输入长度限制

还有很多很多的属性

```js
<div :class="[
  type === 'textarea' ? 'el-textarea' : 'el-input',
  inputSize ? 'el-input--' + inputSize : '',
  {
    'is-disabled': inputDisabled, // disabled 属性
    'is-exceed': inputExceed, // 超出属性
    'el-input-group': $slots.prepend || $slots.append, // 前置后置
    'el-input-group--append': $slots.append, // 后置元素
    'el-input-group--prepend': $slots.prepend, // 前置元素
    'el-input--prefix': $slots.prefix || prefixIcon, // 前置图标
    'el-input--suffix': $slots.suffix || suffixIcon || clearable || showPassword // 展示小图标
  }
  ]"
  @mouseenter="hovering = true"
  @mouseleave="hovering = false"
></div>
```

type 类型不为 textarea

- 处理前置内容使用 $slot.prepend

```js
<template v-if="type !== 'textarea'">
  <!-- 前置元素 -->
  <div class="el-input-group__prepend" v-if="$slots.prepend">
    <slot name="prepend"></slot>
  </div>
  <input
    :tabindex="tabindex"
    v-if="type !== 'textarea'"
    class="el-input__inner"
    v-bind="$attrs"
    :type="showPassword ? (passwordVisible ? 'text': 'password') : type"
    :disabled="inputDisabled"
    :readonly="readonly"
    :autocomplete="autoComplete || autocomplete"
    ref="input"
    @compositionstart="handleCompositionStart"
    @compositionupdate="handleCompositionUpdate"
    @compositionend="handleCompositionEnd"
    @input="handleInput"
    @focus="handleFocus"
    @blur="handleBlur"
    @change="handleChange"
    :aria-label="label"
  >
  <!-- 前置内容 -->
  <span class="el-input__prefix" v-if="$slots.prefix || prefixIcon">
    <slot name="prefix"></slot>
    <i class="el-input__icon"
       v-if="prefixIcon"
       :class="prefixIcon">
    </i>
  </span>
  <!-- 后置内容 -->
  <span
    class="el-input__suffix" // 展示
    v-if="getSuffixVisible()">
    <span class="el-input__suffix-inner">
      <template v-if="!showClear || !showPwdVisible || !isWordLimitVisible">
        <slot name="suffix"></slot>
        <i class="el-input__icon"
          v-if="suffixIcon"
          :class="suffixIcon">
        </i>
      </template>
      <i v-if="showClear" // 后置
        class="el-input__icon el-icon-circle-close el-input__clear"
        @mousedown.prevent
        @click="clear"
      ></i>
      <i v-if="showPwdVisible"
        class="el-input__icon el-icon-view el-input__clear"
        @click="handlePasswordVisible"
      ></i>
      <span v-if="isWordLimitVisible" class="el-input__count">
        <span class="el-input__count-inner">
          {{ textLength }}/{{ upperLimit }}
        </span>
      </span>
    </span>
    <i class="el-input__icon"
      v-if="validateState"
      :class="['el-input__validateIcon', validateIcon]">
    </i>
  </span>
  <!-- 后置元素 -->
  <div class="el-input-group__append" v-if="$slots.append">
    <slot name="append"></slot>
  </div>
</template>
```

主要有几个值得注意的点

- v-bind="$attrs" 就使用组件的时候，可以拿到文本属性
- size 属性

```js
inputSize() {
  return this.size || this._elFormItemSize || (this.$ELEMENT || {}).size;
},
```
- disabled 属性

```js
inputDisabled() {
  return this.disabled || (this.elForm || {}).disabled;
},
```

```js
nativeInputValue() { // 格式化input输入的内容
  return this.value === null || this.value === undefined ? '' : String(this.value);
},
showClear() { // 是否展示 clear 清除按钮，首先 clearable, focused, hovering 必须为 true，disabled,readonly 必须为 false
  return this.clearable &&
    !this.inputDisabled &&
    !this.readonly &&
    this.nativeInputValue &&
    (this.focused || this.hovering);
},
showPwdVisible() { // 是否展示 showPassword 密码框，首先 showPassword, nativeInputValue, focused 必须为 true，disabled,readonly 必须为 false
  return this.showPassword &&
    !this.inputDisabled &&
    !this.readonly &&
    (!!this.nativeInputValue || this.focused);
},
isWordLimitVisible() { // 是否展示 showWordLimit ，首先 showWordLimit, maxlength, focused 必须为 true，disabled,readonly 必须为 false
  return this.showWordLimit &&
    this.$attrs.maxlength &&
    (this.type === 'text' || this.type === 'textarea') &&
    !this.inputDisabled &&
    !this.readonly &&
    !this.showPassword;
},
upperLimit() { // 长度限制
  return this.$attrs.maxlength;
},
textLength() { // 字数长度
  if (typeof this.value === 'number') {
    return String(this.value).length;
  }

  return (this.value || '').length;
},
inputExceed() { // 超出限制
  // show exceed style if length of initial value greater then maxlength
  return this.isWordLimitVisible &&
    (this.textLength > this.upperLimit);
}
```




