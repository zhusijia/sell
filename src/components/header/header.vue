<template>
  <div class="header">
	  {{ msg }}
  </div>
</template>

<script>
export default {
  name: 'v-header',
	data () {
		return {
			msg: "我是header"
		}
	}
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>

</style>
