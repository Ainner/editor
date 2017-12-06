<template>
  <input class="input" @keypress="addInput" @keydown="back" @input="input" @keydown.tab="indentation" />
</template>

<script>
const marked = require('marked')

export default {
  props: ['value'],
  data () {
    return {
      inputValue: ''
    }
  },
  methods: {
    // tab 监听并增加缩进
    indentation (e) {
      e.preventDefault()
    },
    // enter 监听创造往下的 input
    addInput (e) {
      if (e.keyCode === 13) {
        this.$emit('addInput', this.value)
      }
    },
    dasd (e) {
      console.log(e)
    },
    // backspace 监听并达到删除当前的输入框 arrowup & arrowdown 通过键盘进行上下 input 切换
    back (e) {
        // var keyCode = e.keyCode || e.which || e.charCode;
        // var ctrlKey = e.ctrlKey || e.metaKey;
        // console.log(e.keyCode, e.which, e.charCode, e.ctrlKey, e.metaKey, e)
      if (e.keyCode === 8) {
        if (!this.inputValue) {
          e.preventDefault()          // 通过判断 input 为空时，把默认事件去掉，执行删除
          this.$emit('delInput', this.value)
        }
      } else if (e.keyCode === 38) {
        this.$emit('cursor', (this.value - 1))
      } else if (e.keyCode === 40) {
        this.$emit('cursor', (this.value + 1))
      } else if(e.metaKey && e.ctrlKey && e.keyCode === 83) {
        console.log('save');
      }
    },
    // 获取到输入的数值，进行解析传回
    input (e) {
      this.inputValue = e.target.value
      this.$emit('getValue', marked(e.target.value), this.value)
    }
  }
}
</script>

<style scoped>
.input {
  width: 90%;
  font-size: 30px;
  height: 1em;
  padding: 5px 0;
}

.input:hover {
}
</style>
