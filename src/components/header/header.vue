<template>
  <div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img width="64" height="64" :src="seller.avatar" alt="">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div v-if="seller.supports" class="supports">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div>
		</div>
		<div class="bulletin-wrapper"></div>
	</div>
</template>

<script>
export default {
	name: 'v-header',
	props: {
		seller: {
			type: Object
		}
	},
	created(){
		this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
	}
}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
	@import "../../common/stylus/mixin";

	.header
		color #fff
		background #000
		.content-wrapper
			padding 24px 12px 18px 24px
			font-size 0
			.avatar
				display inline-block
				vertical-align top
				img
					border-radius 2px
			.content
				display inline-block
				margin-left 16px
				.title
					margin 2px 0 8px 0
					.brand
						vertical-align top
						width 30px
						height 18px
						display inline-block
						bg-img("brand")
						background-size 30px 18px
						background-repeat no-repeat
					.name
						margin-left 6px
						font-size 16px
						line-height 18px
						font-weight bold
				.description
					margin-bottom 10px
					line-height 12px
					font-size 12px
				.supports
					.icon
						display inline-block
						width 12px
						height 12px
						margin-right 4px
						background-size 12px 12px
						background-repeat no-repeat
						vertical-align top
						&.decrease
							bg-img('decrease_1')
						&.discount	
							bg-img('discount_1')
						&.guarantee
							bg-img('guarantee_1')
						&.invoice
							bg-img('invoice_1')
						&.special
							bg-img('special_1')	
					.text
						line-height 12px
						font-size 12px
</style>	
