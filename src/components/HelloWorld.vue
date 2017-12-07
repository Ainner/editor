<template>
  <div class="all">
    <div class="code">
      <div class="tag">
        <span v-for="(item,index) in inputArr" :key="index">{{ index + 1 }}</span>
      </div>
      <textarea v-show="false"></textarea>
      <my-input v-for="(item,index) in inputArr" :key="index" :value="index" @getValue="getvalue" @addInput="add" @delInput="del" @cursor="changeCursor" ref="input" />
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
      inputArr: ['1'],
      isShow: false,
      value: null,
      hxarr: [[], [], [], [], [], []],
      showArr: [[], [], []]
    }
  },
  methods: {
    changeCursor (v) {
      if (this.inputArr[v] === undefined) {
        console.log('-.-')
      } else {
        this.$refs.input[v].$el.focus()
      }
    },
    add (key, value) {
      if ((this.inputArr.length - 1) === parseInt(key)) {
        this.inputArr.splice(key + 1, 0, '')
        setTimeout(() => {
          this.$refs.input[key + 1].$el.focus()
        }, 0)
      } else {
        this.inputArr.splice(value + 1, 0, '')
        this.$refs.input[key + 1].$el.focus()
      }
    },
    del (value) {
      if (value !== '0') {
        this.$refs.input[value - 1].$el.focus()
        this.inputArr.splice(value, 1)
      }
    },
    getvalue (value, key) {
      this.value = value
      this.$refs.result[key].innerHTML = marked(value)
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
  width: 10%;
  height: auto;
  display: flex;
  align-items: center;
  flex-direction: column;
  color: #000;
}

.tag span {
  font-size: 25px;  
  height: 30px;
  padding: 5px 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.showcase {
  margin-left: 720px;
  width: 720px;
  height: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
  background-image: -webkit-linear-gradient(right, #E8CBC0, #a8CBf0);
}

.resultDiv {
  width: 100%;
  height: auto;
  margin: 0;
  padding: 0;
}
</style>
