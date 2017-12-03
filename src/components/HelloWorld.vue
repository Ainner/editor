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
      <p v-for="(item,index) in inputArr" :key="index" :class="{
        'h1': h1.indexOf(index) !== -1,
        'h2': h2.indexOf(index) !== -1,
        'h3': h3.indexOf(index) !== -1,
        'h4': h4.indexOf(index) !== -1,
        'h5': h5.indexOf(index) !== -1,
        'h6': h6.indexOf(index) !== -1}">{{ item }}</p>
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
      h1: [],
      h2: [],
      h3: [],
      h4: [],
      h5: [],
      h6: []
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
      let x = (hx, num) => {
        v = v.substring(num, v.length)
        this.inputArr.splice(value, 1, v)
        this.h1.splice(value, 1)
        this.h2.splice(value, 1)
        this.h3.splice(value, 1)
        this.h4.splice(value, 1)
        this.h5.splice(value, 1)
        this.h6.splice(value, 1)
        hx.push(value)
      };
      let z = (x, y) => {
        let text = v.slice(0, x) === y && v.charAt(x) !== '#'
        return text
      }
      if (z(1, '#')) {
        x(this.h1, 1)
      } else if (z(2, '##')) {
        x(this.h2, 2)
      } else if (z(3, '###')) {
        x(this.h3, 3)
      } else if (z(4, '####')) {
        x(this.h4, 4)
      } else if (z(5, '#####')) {
        x(this.h5, 5)
      } else if (v.slice(0, 6) === '######') {
        x(this.h6, 6)
      }
      else {
        x()
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

.h2 {
  font-size: 50px;
}

.h3 {
  font-size: 40px;
}

.h4 {
  font-size: 30px;
}

.h5 {
  font-size: 20px;
}

.h6 {
  font-size: 10px;
}
</style>
