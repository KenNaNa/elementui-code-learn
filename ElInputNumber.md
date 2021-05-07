- 基础用法
- 禁用状态
- 步数
- 严格步数
- 精度
- 尺寸
- 按钮位置

```js
watch: {
  value: {
    immediate: true, // 立即执行一次
    handler(value) {
      let newVal = value === undefined ? value : Number(value); // 格式化值
      if (newVal !== undefined) {
        if (isNaN(newVal)) {
          return;
        }

        if (this.stepStrictly) {
          const stepPrecision = this.getPrecision(this.step);
          const precisionFactor = Math.pow(10, stepPrecision); // 算出精度
          newVal = Math.round(newVal / this.step) * precisionFactor * this.step / precisionFactor; // 算出新值
        }

        if (this.precision !== undefined) {
          newVal = this.toPrecision(newVal, this.precision);
        }
      }
      if (newVal >= this.max) newVal = this.max; // 大于最大值则等于最大值
      if (newVal <= this.min) newVal = this.min; // 小于最小值则等于最小值
      this.currentValue = newVal; 
      this.userInput = null;
      this.$emit('input', newVal); // 派发出去
    }
  }
},
```

```js
    computed: {
      minDisabled() { // 小于最小禁止
        return this._decrease(this.value, this.step) < this.min;
      },
      maxDisabled() { // 大于最大禁止
        return this._increase(this.value, this.step) > this.max;
      },
      numPrecision() {
        const { value, step, getPrecision, precision } = this;
        const stepPrecision = getPrecision(step);
        if (precision !== undefined) {
          if (stepPrecision > precision) {
            console.warn('[Element Warn][InputNumber]precision should not be less than the decimal places of step');
          }
          return precision;
        } else {
          return Math.max(getPrecision(value), stepPrecision);
        }
      },
      controlsAtRight() { // 控制
        return this.controls && this.controlsPosition === 'right';
      },
      _elFormItemSize() {
        return (this.elFormItem || {}).elFormItemSize;
      },
      inputNumberSize() {
        return this.size || this._elFormItemSize || (this.$ELEMENT || {}).size;
      },
      inputNumberDisabled() {
        return this.disabled || !!(this.elForm || {}).disabled;
      },
      displayValue() {
        if (this.userInput !== null) {
          return this.userInput;
        }

        let currentValue = this.currentValue;

        if (typeof currentValue === 'number') {
          if (this.stepStrictly) {
            const stepPrecision = this.getPrecision(this.step);
            const precisionFactor = Math.pow(10, stepPrecision);
            currentValue = Math.round(currentValue / this.step) * precisionFactor * this.step / precisionFactor;
          }

          if (this.precision !== undefined) {
            currentValue = currentValue.toFixed(this.precision);
          }
        }

        return currentValue;
      }
    },
```

