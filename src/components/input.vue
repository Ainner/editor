<template>
  <input @keypress="addInput" @keydown="back" @input="input" />
</template>

<script>
export default {
  props: ['value'],
  data () {
    return {
      inputValue: ''
    }
  },
  methods: {
    addInput (e) {
      if (e.code === 'Enter') {
        this.$emit('addInput', this.value)
      }
    },
    back (e) {
      if (e.code === 'Backspace') {
        if (this.inputValue == '') {
          this.$emit('delInput', this.value)
        }
      }
      if (e.code === 'ArrowUp') {
        this.$emit('cursor', (this.value - 1))
      } else if (e.code === 'ArrowDown') {
        this.$emit('cursor', (this.value + 1))
      }
    },
    input (e) {
      if (e.target === '#') {

      }
      this.inputValue = e.target.value
      this.$emit('getValue', e.target.value, this.value)
    }
  }
}
</script>

<style scoped>
input {
  width: 90%;
  font-size: 30px;
  height: 1em;
  outline: none;
  border: none;
  padding: 5px 0;
}
</style>
