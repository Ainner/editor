<template>
  <div class="all">
    <div class="code">
      <div class="tag" @scroll="synchronize" ref="tag">
        <div v-for="(item,index) in inputArr" :key="index"><span>{{ index + 1 }}</span></div>
      </div>
      <textarea @scroll="synchronize" @keydown.tab="indentation" class="text" rows="1" ref="textarea" @input="input" autofocus></textarea>
    </div>
    <div class="showcase">
      <div class="resultDiv" v-for="(item, index) in inputArr" :key="index" ref="result"></div>
    </div>
  </div>
</template>

<script>
import MyInput from './input.vue'
const marked = require('marked')

export default {
  name: 'editor',
  components: {
    'my-input': MyInput
  },
  data () {
    return {
      inputArr: [''],
      value: null,
      row: []
    }
  },
  methods: {
    synchronize (e) {
      this.$refs.tag.scrollTop = e.target.scrollTop
      this.$refs.textarea.scrollTop = e.target.scrollTop
    },
    indentation (e) {
      e.preventDefault()
      let start = e.target.selectionStart
      let end = e.target.selectionEnd
      let arr = this.$refs.textarea.value.split('')
      let newStr = ''
      let add = (num) => {
        for (let i = 0; i < arr.length; i++) {
          newStr = newStr + arr[i]
        }
        this.$refs.textarea.value = num + newStr
        this.$refs.textarea.selectionStart = start + 2
        this.$refs.textarea.selectionEnd = end
        e.target.selectionStart = e.target.selectionStart + 2
      }
      if (!e.target.selectionStart && !this.$refs.textarea.value) {
        console.log(1)
        arr.push('')
        arr[e.target.selectionStart] = arr[e.target.selectionStart] + '  '
        add('')
      } else if (this.$refs.textarea.value && e.target.selectionStart) {
        console.log(2)
        arr[e.target.selectionStart - 1] = arr[e.target.selectionStart - 1] + '  '
        add('')
      } else {
        console.log(3)
        add('  ')
      }
    },
    input (e) {
      this.value = e.target.value
      this.row = this.value.split('\n')
      this.inputArr = this.row
      if (e.inputType !== 'insertLineBreak') {
        for (let i = 0; i < this.inputArr.length; i++) {
          this.$refs.result[i].innerHTML = marked(this.inputArr[i])
        }
      }
    }
  }
}
</script>

<style scoped>
.all {
  width: 100%;
  height: 100%;
  display: flex;
}

.code {
  position: absolute;
  height: 100%;
  width: 720px;
  display: flex;
  align-items: flex-end;
  flex-direction: column;
  overflow-y: scroll;
  background: -webkit-linear-gradient(left, #E8CBC0, #a8CBf0);
}

.tag {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow-y: scroll;
  color: #000;
}

.tag div:first-child {
  border-top: 1px solid #000;
}

.tag div {
  width: 100%;
  font-size: 25px;  
  height: 29px;
  border-bottom: 1px solid #000;
  display: flex;
  align-items: center;
  justify-content: flex-start;
	box-shadow: 0px 0px 2px #999 inset;  
}

.tag span {
  width: 10%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  border-right: 1px solid #999;
}

.showcase {
  margin-left: 720px;
  width: 720px;
  height: 100%;
  display: flex;
  border: 1px solid #000;
  align-items: center;
  flex-direction: column;
  background-image: -webkit-linear-gradient(right, #E8CBC0, #a8CBf0);
}

.resultDiv {
  width: 100%;
  height: auto;
  font-size: 30px;
}

.text {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 2;
  width: 90%;
  font-size: 30px;
  height: 100%;
  white-space: nowrap;
  resize: none;
  line-height: 100%;
}
</style>
