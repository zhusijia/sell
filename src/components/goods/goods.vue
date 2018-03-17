<template>
  <div class="goods">
		<div class="menu-wrapper">
			<ul>
				<li v-for="item in goods" class="menu-item">
					<span class="text border-1px">
						<span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="goods-wrapper">
			<div v-for="item in goods" class="food-list">
				<h1 class="list-title">
					{{item.name}}
				</h1>
				<ul>
					<li v-for="food in item.foods" class="food-detail border-1px">
						<div class="icon">
							<img :src="food.icon" width="57" height="57">
						</div>
						<div class="cotent">
							<h2 class="name">{{food.name}}</h2>
							<p class="desc">{{food.description}}</p>
							<div class="extra">
								<span class="sell-count">月售{{food.sellCount}}</span>
								<span class="rating">好评率{{food.rating}}%</span>
							</div>
							<div class="price">
								<span>{{food.price}}</span>
								<span v-if="food.oldPrice">{{food.oldPrice}}</span>
							</div>
						</div>
					</li>
				</ul>
			</div>
		</div>
  </div>
</template>

<script>
	const ERR_OK = 0;
	export default {
		name: 'v-goods',
		props: {
			seller: {
				type: Object
			}
		},
		data () {
			return {
				goods: {},
				classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
			}
		},
		created() {
			this.$http.get('/api/goods').then(response => {
				response = response.body;
				if(response.errno === ERR_OK){
					this.goods = response.data;
				}
				console.log(this.goods)
			})
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
	@import "../../common/stylus/mixin";
	.goods
		display flex
		position absolute
		top 174px
		bottom 46px
		width 100%
		overflow hidden
		.menu-wrapper
			flex 0 0 80px
			width 80px
			background #f3f5f7
			.menu-item
				display table
				height 54px
				width 56px
				line-height 14px
				padding 0 12px
				.text
					display table-cell
					width 56px
					vertical-align middle
					font-size 12px
					border-1px(rgba(7, 17, 27, 0.1))
					.icon
						display inline-block
						width 12px
						height 12px
						margin-right 2px
						background-size 12px 12px
						background-repeat no-repeat
						vertical-align top
						&.decrease
							bg-img('decrease_3')
						&.discount	
							bg-img('discount_3')
						&.guarantee
							bg-img('guarantee_3')
						&.invoice
							bg-img('invoice_3')
						&.special
							bg-img('special_3')	
		.goods-wrapper
			flex 1
			background #fff
			.list-title
				font-size 12px
				color rgb(147, 153, 169)
				line-height 26px
				height 26px
				border-left 2px solid #d9dde1
				padding-left 12px
				background #f3f5f7
			.food-detail
				display flex
				background #fff;
				margin 0 18px
				padding 18px 0
				border-1px(rgba(7, 17, 27, 0.1))
				&:last-child
					border-none()
				.icon
					flex 0 0 57px
					margin-right 10px
				.cotent
					flex 1
					.name
						font-size 14px
						line-height 14px
						color rgb(7, 17, 27)
						margin-top 2px
					.desc
						font-size 10px
						line-height 10px
						color rgb(147, 153, 159)
						margin-top 8px
					.extra
						font-size 0
						color rgb(147, 153, 159)
						margin-top 8px
						.sell-count
							font-size 10px
							line-height 10px
						.rating
							font-size 10px
							line-height 10px
</style>
