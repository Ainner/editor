<template>
  <input class="input" @mouseout="getCoverText" @keypress="addInput" @keydown="back" @input="input" @keydown.tab="indentation" :value="this.inputValue" />
</template>

<script>
export default {
  props: ['value'],
  data () {
    return {
      inputValue: '',
      coverText: ''
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
    getCoverText () {
      let a = window.getSelection().toString()
      console.log(window.getSelection())
      if (a !== null && a.trim() !== '') {
        this.coverText = a
      }
    },
    back (e) {
      if (e.keyCode === 8) {
        if (!this.inputValue) {
          e.preventDefault()          // 通过判断 input 为空时，把默认事件去掉，执行删除
          this.$emit('delInput', this.value)
        }
      } else if (e.keyCode === 38) {
        this.$emit('cursor', (this.value - 1))
      } else if (e.keyCode === 40) {
        this.$emit('cursor', (this.value + 1))
      } else if (e.metaKey && e.ctrlKey && e.keyCode === 83) {
        this.getCoverText()
        let a = '**'
        let ary = this.inputValue.split(this.coverText)
        if (ary.length > 1 && ary.length < 3) {
          for (let i = 0; i < ary.length; i++) {
            ary[i] = a + ary[i] + a
          }
          this.inputValue = ary.join(this.coverText)
          this.inputValue = this.inputValue.substring(0, this.inputValue.length - 2)
          this.inputValue = this.inputValue.substring(2, this.inputValue.length)
        }
        this.$emit('getValue', this.inputValue, this.value)
      }
    },
    // 获取到输入的数值，进行解析传回
    input (e) {
      this.inputValue = e.target.value
      this.$emit('getValue', this.inputValue, this.value)
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
