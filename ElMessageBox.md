- title MessageBox 标题	
- message MessageBox 消息正文内容
- dangerouslyUseHTMLString 是否将 message 属性作为 HTML 片段处理
- type 消息类型，用于显示图标
- iconClass 自定义图标的类名，会覆盖 type
- customClass MessageBox 的自定义类名
- callback 若不使用 Promise，可以使用此参数指定 MessageBox 关闭后的回调
- showClose 	MessageBox 是否显示右上角关闭按钮
- beforeClose MessageBox 关闭前的回调，会暂停实例的关闭
- distinguishCancelAndClose 是否将取消（点击取消按钮）与关闭（点击关闭按钮或遮罩层、按下 ESC 键）进行区分
- lockScroll 是否在 MessageBox 出现时将 body 滚动锁定	
- showCancelButton 是否显示取消按钮	
- showConfirmButton 是否显示确定按钮	
- ancelButtonText	取消按钮的文本内容	string	—	取消
- confirmButtonText	确定按钮的文本内容	string	—	确定
- cancelButtonClass	取消按钮的自定义类名	string	—	—
- confirmButtonClass	确定按钮的自定义类名	string	—	—
- closeOnClickModal	是否可通过点击遮罩关闭 MessageBox	boolean	—	true（以 alert 方式调用时为 false）
- closeOnPressEscape	是否可通过按下 ESC 键关闭 MessageBox	boolean	—	true（以 alert 方式调用时为 false）
- closeOnHashChange	是否在 hashchange 时关闭 MessageBox	boolean	—	true
- showInput	是否显示输入框	boolean	—	false（以 prompt 方式调用时为 true）
- inputPlaceholder	输入框的占位符	string	—	—
- inputType	输入框的类型	string	—	text
- inputValue	输入框的初始文本	string	—	—
- inputPattern	输入框的校验表达式	regexp	—	—
- inputValidator	输入框的校验函数。可以返回布尔值或字符串，若返回一个字符串, 则返回结果会被赋值给 inputErrorMessage	function	—	—
- inputErrorMessage	校验未通过时的提示文本	string	—	输入的数据不合法!
- center	是否居中布局	boolean	—	false
- roundButton	是否使用圆角按钮	boolean	—	false

```html
<template>
  <transition name="msgbox-fade">
    <div
      class="el-message-box__wrapper"
      tabindex="-1"
      v-show="visible"
      @click.self="handleWrapperClick" 
      role="dialog"
      aria-modal="true"
      :aria-label="title || 'dialog'">
      <div class="el-message-box" :class="[customClass, center && 'el-message-box--center']">
        <div class="el-message-box__header" v-if="title !== null">
          <div class="el-message-box__title">
            <div
              :class="['el-message-box__status', icon]"
              v-if="icon && center">
            </div>
            <span>{{ title }}</span>
          </div>
          <button
            type="button"
            class="el-message-box__headerbtn"
            aria-label="Close"
            v-if="showClose"
            @click="handleAction(distinguishCancelAndClose ? 'close' : 'cancel')"
            @keydown.enter="handleAction(distinguishCancelAndClose ? 'close' : 'cancel')">
            <i class="el-message-box__close el-icon-close"></i>
          </button>
        </div>
        <div class="el-message-box__content">
          <div class="el-message-box__container">
            <div
              :class="['el-message-box__status', icon]"
              v-if="icon && !center && message !== ''">
            </div>
            <div class="el-message-box__message" v-if="message !== ''">
              <slot>
                <p v-if="!dangerouslyUseHTMLString">{{ message }}</p>
                <p v-else v-html="message"></p>
              </slot>
            </div>
          </div>
          <div class="el-message-box__input" v-show="showInput">
            <el-input
              v-model="inputValue"
              :type="inputType"
              @keydown.enter.native="handleInputEnter"
              :placeholder="inputPlaceholder"
              ref="input"></el-input>
            <div class="el-message-box__errormsg" :style="{ visibility: !!editorErrorMessage ? 'visible' : 'hidden' }">{{ editorErrorMessage }}</div>
          </div>
        </div>
        <div class="el-message-box__btns">
          <el-button
            :loading="cancelButtonLoading"
            :class="[ cancelButtonClasses ]"
            v-if="showCancelButton"
            :round="roundButton"
            size="small"
            @click.native="handleAction('cancel')"
            @keydown.enter="handleAction('cancel')">
            {{ cancelButtonText || t('el.messagebox.cancel') }}
          </el-button>
          <el-button
            :loading="confirmButtonLoading"
            ref="confirm"
            :class="[ confirmButtonClasses ]"
            v-show="showConfirmButton"
            :round="roundButton"
            size="small"
            @click.native="handleAction('confirm')"
            @keydown.enter="handleAction('confirm')">
            {{ confirmButtonText || t('el.messagebox.confirm') }}
          </el-button>
        </div>
      </div>
    </div>
  </transition>
</template>
```


