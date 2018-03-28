<template>
	<div class="shop-cart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo">
						<span class="ion icon-shopping_cart"></span>
					</div>
				</div>
				<div class="price">¥0</div>
				<div class="desc">另需配送费¥0元	</div>
			</div>
			<div class="content-right">¥0元起送</div>
		</div>
	</div>
</template>
<script>
export default {
	name: "shopCarts",
	created () {

	},
	computed: {

	},
	method: {

	}
}
</script>
<style lang="stylus" scoped>
	.shop-cart
		position fixed
		bottom 0
		height 46px
		width 100%
		background #000
		z-index 10;
		.content
			display flex
			height 100%
			background #141d27
			.content-left
				flex 1
				font-size 0
				.logo-wrapper
					box-sizing border-box
					display inline-block
					position relative
					top -10px
					margin 0 12px
					padding 6px
					height 56px
					width 56px
					border-radius 50%
					background #141d27
					.logo
						width 100%
						height 100%
						border-radius 50%
						background #2b343c
						text-align center
						.icon-shopping_cart
							font-size 24px
							line-height 46px
							color rgba(255, 255, 255, 0.4)
				.price
					display inline-block
					vertical-align top
					color rgba(255, 255, 255, 0.4)
					font-size 16px
					font-weight 700
					line-height 24px
					margin 12px 0
					padding-right 12px
					box-sizing border-box
					border-right 1px solid rgba(255, 255, 255, 0.1)
				.desc
					display inline-block
					vertical-align top
					margin 12px 0
					padding 0 12px
					color rgba(255, 255, 255, 0.4)
					font-size 14px
					font-weight 300
					line-height 24px
			.content-right
				flex 0 0 105px
				box-sizing border-box
				width 105px
				color rgba(255, 255, 255, 0.4)
				font-size 12px
				font-weight 700
				line-height 46px

</style>
