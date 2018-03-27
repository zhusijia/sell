<template>
  <div class="goods">
		<div class="menu-wrapper" ref="menuWrapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index, $event)">
					<span class="text border-1px">
						<span v-if="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
					</span>
				</li>
			</ul>
		</div>
		<div class="goods-wrapper" ref="goodsWrapper">
			<div>
				<div v-for="item in goods" class="food-list food-list-hook">
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
									<span class="now">¥{{food.price}}</span>
									<span v-if="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
								</div>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</div>
		<shopCarts></shopCarts>
  </div>
</template>

<script>
	import BScroll from 'better-scroll';
	import shopCarts from "../../components/shopcarts/shopcarts"

	const ERR_OK = 0;
	export default {
		name: 'v-goods',
		components: {
			shopCarts: shopCarts
		},
		props: {
			seller: {
				type: Object
			}
		},
		data () {
			return {
				goods: {},
				listHeight: [],
				scrollY: 0
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
			this.$http.get('/api/goods').then(response => {
				response = response.body;
				if(response.errno === ERR_OK){
					this.goods = response.data;
					this.$nextTick(()=> {
						this._initScroll();
						this._calculateHeight();
					})
				}
			})
		},
		computed: {
		 currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      }
		},
		methods: {
			selectMenu(index, event) {
				let goodsList = this.$refs.goodsWrapper.getElementsByClassName("food-list-hook");
				let el = goodsList[index];
				this.goodsScroll.scrollToElement(el, 300);
			},
			_initScroll() {
				this.menuScroll = new BScroll(this.$refs.menuWrapper, {
					click: true
				})
				this.goodsScroll = new BScroll(this.$refs.goodsWrapper, {
					probeType: 3
				});
				this.goodsScroll.on('scroll', (pos)=>{
					this.scrollY = Math.abs(Math.round(pos.y));
				})
			},
			_calculateHeight() {
				let goodsList = this.$refs.goodsWrapper.getElementsByClassName("food-list-hook");
				let height = 0;
				this.listHeight.push(height);
				for(let i = 0;i < goodsList.length; i++ ) {
					let item = goodsList[i]
					height += item.clientHeight;
					this.listHeight.push(height);
				}
				console.log("this.listHeight", this.listHeight)
			}
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
				&.current
					position relative
					background-color #fff;
					margin-top -1px
					z-index 10
					font-weight 700
					.text
						border-none()
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
						line-height 12px
						color rgb(147, 153, 159)
						margin-top 8px
					.extra
						font-size 0
						color rgb(147, 153, 159)
						margin-top 8px
						.sell-count
							font-size 10px
							line-height 10px
							margin-right: 12px;
						.rating
							font-size 10px
							line-height 10px
					.price
						font-size 24px
						.now
							margin-right 8px
							font-size 14px
							font-weight 700
							color rgb(240, 20, 20)
						.old
							text-decoration line-through
							font-size 10px
							font-weight 700
							color rgb(147, 153, 159)
</style>
