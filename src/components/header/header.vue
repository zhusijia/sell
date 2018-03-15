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
			<div class="support-count" @click="showDetail">
				<span v-if="seller.supports" class="count">{{seller.supports.length}}个</span>
				<i class="ion icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<div class="bulletin-wrapper" @click="showDetail">
			<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
			<i class="ion icon-keyboard_arrow_right"></i>
		</div>
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>
		<div v-show="detailShow" class="detail">
			<div class="detail-wrapper clearfix">
				<div class="detail-content">
					<h1 class="name">{{seller.name}}</h1>
					<div class="star-wrapper">
						<v-star :size="48" :score="seller.score"></v-star>
					</div>
					<div class="title">
						<div class="line"></div>
						<div class="text">优惠信息</div>
						<div class="line"></div>
					</div>
				</div>
			</div>
			<div class="detail-close">
				<i class="icon-close" @click="hideDetail"></i>
			</div>
		</div>
	</div>
</template>

<script>
	import star from '../../components/star/star'
	export default {
		name: 'v-header',
		props: {
			seller: {
				type: Object
			}
		},
		created(){
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		},
		data(){
			return {
				detailShow: false
			}
		},
		methods: {
			showDetail() {
				this.detailShow = true;
			},
			hideDetail() {
				this.detailShow = false;
			}
		},
		components: {
			"v-star": star
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
	@import "../../common/stylus/mixin";

	.header
		position relative
		color #fff
		background rgba(7, 17, 27, 0.5)
		overflow hidden
		.content-wrapper
			padding 24px 12px 18px 24px
			font-size 0
			position relative
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
			.support-count
				position absolute
				right 12px
				bottom 14px
				padding 7px 8px
				height 12px
				line-height 12px
				border-radius 14px
				background-color rgba(0, 0, 0, 0.2)
				text-align center
				.count
					font-size 10px
					color rgb(255, 255, 255)
					margin-right 2px
				.ion
					font-size 10px
					heigh 12px
					line-height 12px
		.bulletin-wrapper
			height 28px
			line-height 28px
			padding 0 22px 0 12px
			white-space nowrap
			overflow hidden
			text-overflow ellipsis
			position relative
			background rgba(7, 17, 27, 0.2)
			.bulletin-title
				vertical-align top
				margin-top 8px
				width 22px
				height 12px
				display inline-block
				bg-img('bulletin')
				background-size 22px 12px
				background-repeat no-repeat
			.bulletin-text
				vertical-align top
				margin 0 4px
				font-size 10px
			.ion
				position absolute
				top 8px
				right 12px
				font-size 10px
		.background
			position absolute
			z-index -1
			top 0
			left 0
			width 100%
			height 100%
			filter blur(10px)
		.detail
			position fixed
			top 0
			left 0
			z-index 100
			width 100%
			height 100%
			overflow auto
			background rgba(7, 17, 27, 0.8)
			.detail-wrapper
				min-height 100%
				height auto
				width 100%
				.detail-content
					margin-top 64px
					padding-bottom 64px
					.name
						line-height 16px
						text-align center
						font-weight 700
						font-size 16px
					.star-wrapper
						margin-top 18px
						text-align center
						padding 2px 0
					.title
						display flex
						width 80%
						margin 30px auto 24px auto 
						.line
							flex 1
							margin-bottom: 8px;
							border-bottom 1px solid rgba(255, 255, 255, 0.2)
			.detail-close
				text-align center
				position relative
				margin -64px auto 0 auto
				width 32px
				height 32px
</style>	
