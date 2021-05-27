<template>
  <div class="beauty-tags__wrapper">
    <template v-for="(tag, index) in defaultTags">
      <a-tooltip v-if="tag.name.length > 20" :key="tag.id" :title="tag.name">
        <a-tag
          :key="tag.id"
          :closable="index !== 0"
          :class="{ 'active-tag': index === curIndex }"
          @close="() => handleClose(tag)"
          @click="handleClick(tag, index)"
          @dblclick="handleModify(tag, index)"
        >
          {{ `${tag.name.slice(0, 20)}...` }}
        </a-tag>
      </a-tooltip>
      <a-tag
        v-else
        :key="tag.id"
        :closable="index !== 0"
        :class="{ 'active-tag': index === curIndex }"
        @close="() => handleClose(tag)"
        @click="handleClick(tag, index)"
        @dblclick="handleModify(tag, index)"
      >
        {{ tag.name }}
      </a-tag>
    </template>
    <a-input
      v-if="inputVisible"
      ref="input"
      type="text"
      size="small"
      :style="{ width: '78px' }"
      :value="inputValue"
      @change="handleInputChange"
      @blur="handleInputConfirm"
      @keyup.enter="handleInputConfirm"
    />
    <a-tag
      v-else
      style="background: #fff; borderstyle: dashed"
      @click="showInput"
    >
      添加小类
    </a-tag>
  </div>
</template>
<script>
export default {
  name: "beautyTags",
  props: {
    tags: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      defaultTags: [],
      inputVisible: false,
      inputValue: "",
      modifyTag: null,
      curIndex: 0,
      type: "add",
    };
  },
  watch: {
    tags: {
      handler(val) {
        this.defaultTags = val || []
      },
      deep: true,
      immediate: true
    }
  },
  methods: {
    handleModify(tag, index) {
      this.modifyTag = tag;
      this.curIndex = index;
      this.inputValue = tag.name;
      this.inputVisible = true;
      this.type = "modify";
    },
    handleClick(tag, index) {
      this.curIndex = index;
      // 1. 请求数据
    },
    handleClose(removedTag) {
      const tags = this.defaultTags.filter((tag) => tag.id !== removedTag.id);
      this.defaultTags = tags;
    },

    showInput() {
      this.inputVisible = true;
      this.$nextTick(function () {
        this.$refs.input.focus();
      });
    },

    handleInputChange(e) {
      this.inputValue = e.target.value;
    },

    handleInputConfirm() {
      // 1. 添加的时候，请求接口
      // 2. 修改的时候，请求接口
      const inputValue = this.inputValue;
      let defaultTags = this.defaultTags;
      let names = defaultTags.map((tag) => tag.name);
      let tagIds = defaultTags.map((tag) => tag.id);
      let flag = false
      let flagIndex = null
      if (this.type === "add") {
        if (inputValue && names.indexOf(inputValue) === -1) {
          defaultTags = [...defaultTags, { id: defaultTags.length, name: inputValue }];
        }
      } else if (this.type === "modify" && this.modifyTag) {
        if (inputValue && tagIds.indexOf(this.modifyTag.id) !== -1) {
          for (let i = 0; i < defaultTags.length; i++) {
            if (defaultTags[i].id === this.modifyTag.id) {
              flag = true
              flagIndex = i
              defaultTags[i].name = inputValue;
              break;
            }
          }
        }
      }
      Object.assign(this, {
        defaultTags,
        inputVisible: false,
        inputValue: "",
      });
      this.$emit('update:tags', this.defaultTags)
      if(this.type === 'modify' && flag && inputValue) {
        this.$emit('modify-tag', inputValue, this.defaultTags[flagIndex])
      } else {
        this.$emit('add-tag', inputValue)
      }
    },
  },
};
</script>

<style lang="less" scoped>
.active-tag {
  background: #1890ff;
  color: #fff;
  ::v-deep .anticon.anticon-close {
    color: #fff;
  }
}
</style>
