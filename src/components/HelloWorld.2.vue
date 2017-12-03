<template>
  <div class="all">
    <div class="code">
      <div class="tag">
        <span v-for="(item,index) in inputArr" :key="index">#{{ index + 1 }}</span>
      </div>
      <textarea v-show="false"></textarea>
      <my-input v-for="(item,index) in inputArr" :key="index" :value="index" @addInput="add" @delInput="del" @getValue="importValue" @cursor="changeCursor" ref="des" />
    </div>
    <div class="showcase">
      <p v-for="(item,index) in inputArr" :key="index" :class="{'h1': medium.indexOf(index) !== -1}">{{ item }}</p>
    </div>
  </div>
</template>

<script>
import MyInput from './input.vue'

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
      medium: []
    }
  },
  methods: {
    changeCursor (v) {
      if (this.inputArr[v] == undefined) {
        console.log('-.-')
      } else {
        this.$refs.des[v].$el.focus()
      }
    },
    add (value) {
      // this.inputArr.splice(value + 1, 0, '2')
      if ((this.inputArr.length - 1) === parseInt(value)) {
        this.inputArr.splice(value + 1, 0, '')
        setTimeout(() => {
          this.$refs.des[value + 1].$el.focus()
        }, 0)
      } else {
        this.inputArr.splice(value + 1, 0, '')
        this.$refs.des[value + 1].$el.focus()
      }
    },
    del (value) {
      if (value != '0') {
        this.$refs.des[value - 1].$el.focus()
        this.inputArr.splice(value, 1)
      }
    },
    importValue (v, value) {
      if (v.charAt(0) === '#' && v.charAt(1) !== '#') {
        console.log(1)
        v = v.substr(1)
        this.inputArr.splice(value, 1, v)
        this.medium.push(value)
      } else if (v.slice(0, 2) === '##' && v.charAt(2) !== '#') {
        console.log(2)
        console.log(v.substring(3, v.length - 3))
        v = v.substr(1)
        v = v.substr(2)
        this.inputArr.splice(value, 1, v)
      } else if (v.slice(0, 3) === '###' && v.charAt(3) !== '#') {

      }
      else {
        this.inputArr.splice(value, 1, v)
      }
      this.value = v
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
}

.tag {
  position: absolute;
  left: 0;
  width: 10%;
  height: auto;
  display: flex;
  align-items: center;
  flex-direction: column;
  background: #f0f;
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
}

.showcase p {
  width: 100%;
  background: #f0f00f;
  margin: 0;
  padding: 0;
}

.h1 {
  font-size: 60px;
}
</style>
