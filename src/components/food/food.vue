<template>
	<transition name="move">
		<div v-show="showFlag" class="food" ref="food">
			<div class="container">
				<div class="food-content">
					<div class="image-header">
						<img :src="food.image">
						<div class="back" @click="hide">
							<i class="ion icon-arrow_lift"></i>
						</div>
					</div>
				</div>
				<div class="cotent border-1px">
					<h2 class="name">{{food.name}}</h2>
					<div class="extra">
						<span class="sell-count">月售{{food.sellCount}}</span>
						<span class="rating">好评率{{food.rating}}%</span>
					</div>
					<div class="price-container">
						<div class="price">
							<span class="now">¥{{food.price}}</span>
							<span v-if="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
						</div>
						<div class="cartcontrol-wrapper">
							<cartControl :food=food @cart-add="addFood"></cartControl>
						</div>
						<div class="buy" @click.stop.prevent="addFirst" v-show="!food.count || food.count === 0">加入购物车</div>
					</div>
				</div>
				<div class="food-description border-1px">
					<h2 class="title">
						商品介绍
					</h2>
					<div class="description">
						{{food.info}}
					</div>
				</div>
				<div class="food-ratings">
					<div></div>
				</div>
			</div>
		</div>
	</transition>
</template>

<script>
import BScroll from "better-scroll";
import Vue from "vue";
import cartControl from "../../components/cartcontrol/cartcontrol";

export default {
  name: "v-foods",
  props: {
    food: {
      type: Object
    }
  },
  data() {
    return {
      showFlag: false
    };
  },
  components: {
    cartControl: cartControl
  },
  methods: {
    show() {
      this.showFlag = true;
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.food, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      });
    },
    hide() {
      this.showFlag = false;
		},
		addFood(target) {
			console.log(target)
			// 触发的是父元素上面定义的事件
			this.$emit('cart-add', event.target);
		},
		addFirst(event) {
			console.log(event)
			// 触发的是父元素上面定义的事件
			this.$emit('cart-add', event.target);
			Vue.set(this.food, 'count', 1)
		}
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
@import '../../common/stylus/mixin';

.food {
	position: fixed;
	left: 0;
	top: 0;
	bottom: 46px;
	z-index: 30;
	width: 100%;
	background: #f3f5f7;
	overflow: auto;

	&.move-enter, &.move-leave-to {
		transform: translate3d(100%, 0, 0);
	}

	&.move-enter-active, &.move-leave-active {
		transition: all 0.2s linear;
	}

	.image-header {
		position: relative;
		width: 100%;
		height: 0;
		padding-top: 100%;

		img {
			position: absolute;
			top: 0;
			left: 0;
			width: 100%;
			height: 100%;
		}

		.back {
			position: absolute;
			top: 10px;
			left: 0;

			.ion {
				display: block;
				padding: 10px;
				font-size: 20px;
				color: #fff;
			}
		}
	}

	.cotent {
		background: #ffffff;
		padding: 18px;
		border-1px(rgba(7, 17, 27, 0.1));

		.name {
			font-size: 14px;
			font-weight: 700;
			color: rgb(7, 17, 27);
			line-height: 14px;
			margin-bottom: 8px;
		}

		.extra {
			font-size: 10px;
			color: rgb(147, 153, 159);
			line-height: 10px;
			margin-bottom: 18px;

			.sell-count {
				margin-right: 12px;
			}
		}

		.price-container {
			display: flex;
			justify-content: space-between;
			flex-direction: row;
		}

		.price {
			.now {
				margin-right: 8px;
				font-size: 16px;
				font-weight: 700;
				color: #f01414;
			}

			.old {
				text-decoration: line-through;
				font-size: 10px;
				font-weight: 700;
				color: #93999f;
			}
		}

		.cartcontrol-wrapper {
			position: absolute;
			right: 20px;
			top: 58px;
		}

		.buy {
			font-size: 10px;
			color: #fff;
			background: rgb(0, 160, 220);
			line-height: 12px;
			border-radius: 12px;
			padding: 6px 12px;
			position: absolute;
			top: 64px;
			right: 18px;
		}
	}

	.food-description {
		background: #ffffff;
		padding: 18px;
		margin-top: 16px;
		border-top-1px(rgba(7, 17, 27, 0.1));
		border-1px(rgba(7, 17, 27, 0.1));

		.title {
			font-size: 14px;
			line-height: 14px;
		}

		.description {
			font-size: 12px;
			font-weight: 200;
			color: rgb(77, 85, 93);
			line-height: 24px;
			margin-top: 6px;
			padding: 0 8px;
		}
	}
}
</style>
