<template>
  <div class="all">
      <p v-show="show[0].indexOf(value) !== -1" class="fontSize" :class="{
      'h1': arr[0].indexOf(value) !== -1,
      'h2': arr[1].indexOf(value) !== -1,
      'h3': arr[2].indexOf(value) !== -1,
      'h4': arr[3].indexOf(value) !== -1,
      'h5': arr[4].indexOf(value) !== -1,
      'h6': arr[5].indexOf(value) !== -1}">{{ msg }}</p>
      <ul v-show="show[1].indexOf(value) !== -1">
        <li>{{ show[1].indexOf(value) !== -1 }}</li>
      </ul>
  </div>
</template>

<script>
	export default {
		name: 'customize',
    props: ['value', 'msg', 'arr', 'show'],
		data () {
			return {
			}
		},
		methods: {
		}
	}
</script>

<style scoped>
.all {
  width: 100%;
  height: auto;
}

.fontSize {
  width: 100%;
  background: #f0f00f;
  margin: 0;
  padding: 0;
}
</style>