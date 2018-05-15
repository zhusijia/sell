<template>
	<div class="shop-cart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper" @click="toggleList">
					<div class="logo" :class="{'high-light': totalCount}">
						<span class="ion icon-shopping_cart"></span>
					</div>
					<transition name="fade">
						<span v-if="totalCount" class="total-count">{{totalCount}}</span>
					</transition>
				</div>
				<div class="price" :class="{'high-light': totalPrice}">¥{{totalPrice}}</div>
				<div class="desc">另需配送费¥{{deliveryPrice}}元	</div>
			</div>
			<div class="content-right">
				<!-- <div class="pay" :class="{'high-light': totalPrice >= minPrice}">
					<span v-if="totalPrice === 0">¥{{minPrice}}元起送</span>
					<span v-if="deliverPrice > 0">还差¥{{deliverPrice}}起送</span>
					<span v-else>去结算</span>
				</div> -->
				<div class="pay" :class="{'high-light': totalPrice >= minPrice}">
					{{payDesc}}
				</div>
			</div>
			<div class="ball-container">
				<div v-for="ball in balls">
					<transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
						<div v-show="ball.show" class="ball">
							<div class="inner inner-hook"></div>
						</div>
					</transition>
				</div>
			</div>
			<transition name="translate">
				<div class="shopcart-list" v-show="listShow">
					<div class="list-header">
						<h1 class="title">购物车</h1>
						<span class="empty" @click="emptyFoods">清空</span>
					</div>
					<div class="list-content" ref="listContent">
						<ul>
							<li class="food" v-for="food in mySelectFoods">
								<span class="name">{{food.name}}</span>
								<div class="price">
									<span>¥{{food.price*food.count}}</span>
								</div>
								<div class="cart-control">
									<cartControl :food=food @cart-add="drop"></cartControl>
								</div>
							</li>
						</ul>
					</div>
				</div>
			</transition>
		</div>
	</div>
</template>
<script>
import cartControl from "../../components/cartcontrol/cartcontrol";
import BScroll from "better-scroll";

const ERR_OK = 0;
export default {
  name: "shopCarts",
  components: {
    cartControl: cartControl
  },
  watch: {
    selectFoods(newValue, oldValue) {
      this.mySelectFoods = newValue;
    }
  },
  data() {
    return {
      seller: {},
      balls: [
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        },
        {
          show: false
        }
      ],
      dropBalls: [],
      mySelectFoods: this.selectFoods,
      fold: true
    };
  },
  created() {
    this.$http.get("/api/seller").then(response => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.seller = response.data;
        // this.$nextTick(() => {
        //   this._initScroll();
        // });
      }
    });
  },
  props: {
    selectFoods: {
      type: Array,
      default() {
        return [];
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  computed: {
    totalPrice() {
      let total = 0;
      _.each(this.selectFoods, food => {
        total += food.price * food.count;
      });
      return total;
    },
    totalCount() {
      let total = 0;
      _.each(this.selectFoods, food => {
        total += food.count;
      });
      return total;
    },
    deliverPrice() {
      let deliverPrice = this.seller.minPrice - this.totalPrice;
      return deliverPrice;
    },
    payDesc() {
      let deliverPrice = this.seller.minPrice - this.totalPrice;
      if (this.totalPrice == 0) {
        return `¥${this.minPrice}元起送`;
      } else if (deliverPrice > 0) {
        return `还差¥${deliverPrice}元起送`;
      } else {
        return `去结算`;
      }
    },
    listShow() {
      console.log("this.totalCount", this.totalCount);
      if (!this.totalCount) {
        this.fold = true;
        return false;
      }
      let show = !this.fold;
      if (show) {
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.listContent, {
            click: true
          });
        });
      }
      return show;
    }
  },
  methods: {
    toggleList() {
      if (!this.totalCount) {
        return;
      }
      this.fold = !this.fold;
    },
    _initScroll() {
      this.listScroll = new BScroll(this.$refs.listContent, {
        click: true
      });
    },
    emptyFoods() {
      this.selectFoods.forEach(food => {
        food.count = 0;
      });
    },
    addFood(target) {
      this.drop(target);
    },
    drop(el) {
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i];
        if (!ball.show) {
          ball.show = true;
          ball.el = el;
          this.dropBalls.push(ball);
          return;
        }
      }
    },
    beforeDrop(el) {
      // 优化原先算法
      let ball = this.dropBalls[0];
      let rect = ball.el.getBoundingClientRect();
      let x = rect.left - 32 + 10;
      let y = -(window.innerHeight - rect.top - 22 - 24);
      el.style.webkitTransform = `translate3d(0, ${y}px, 0)`;
      el.style.transfrom = `translate3d(0, ${y}px, 0)`;
      let inner = el.getElementsByClassName("inner-hook")[0];
      inner.style.webkitTransform = `translate3d(${x}px, 0, 0)`;
      inner.style.transfrom = `translate3d(${x}px, 0, 0)`;
    },
    dropping(el, done) {
      // 触发浏览器重绘，避免样式合并
      let rf = el.offsetHeight;
      this.$nextTick(() => {
        el.style.webkitTransform = "translate3d(0,0,0)";
        el.style.transform = "translate3d(0,0,0)";
        let inner = el.getElementsByClassName("inner-hook")[0];
        inner.style.webkitTransform = "translate3d(0,0,0)";
        inner.style.transform = "translate3d(0,0,0)";
        el.addEventListener("transitionend", done);
      });
    },
    afterDrop(el) {
      let ball = this.dropBalls.shift();
      if (ball) {
        ball.show = false;
        el.style.display = "none";
      }
    }
  }
};
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin';

.shop-cart {
	position: fixed;
	bottom: 0;
	height: 46px;
	width: 100%;
	background: #000;
	z-index: 10;

	.content {
		display: flex;
		height: 100%;
		background: #141d27;

		.content-left {
			flex: 1;
			font-size: 0;

			.logo-wrapper {
				box-sizing: border-box;
				display: inline-block;
				position: relative;
				top: -10px;
				margin: 0 12px;
				padding: 6px;
				height: 56px;
				width: 56px;
				border-radius: 50%;
				background: #141d27;
				z-index: 100;

				.logo {
					width: 100%;
					height: 100%;
					border-radius: 50%;
					background: #2b343c;
					text-align: center;
					color: rgba(255, 255, 255, 0.4);

					&.high-light {
						color: #ffffff;
						background: #0099cc;
					}

					.icon-shopping_cart {
						font-size: 24px;
						line-height: 46px;
					}

					.fade-enter, .fade-leave-to {
						opacity: 0;
					}

					.fade-enter-active, .fade-leave-active {
						transition: all 0.2s linear;
					}
				}

				.total-count {
					box-sizing: border-box;
					position: absolute;
					top: 0px;
					right: 0px;
					width: 24px;
					font-size: 9px;
					color: #fff;
					line-height: 16px;
					text-align: center;
					background: rgb(240, 20, 20);
					box-shadow: 0 4px 8px 0px rgba(0, 0, 0, 0.4);
					border-radius: 30px;
					padding: 0 6px;
				}
			}

			.price {
				display: inline-block;
				vertical-align: top;
				color: rgba(255, 255, 255, 0.4);
				font-size: 16px;
				font-weight: 700;
				line-height: 24px;
				margin: 12px 0;
				padding-right: 12px;
				box-sizing: border-box;
				border-right: 1px solid rgba(255, 255, 255, 0.1);

				&.high-light {
					color: #fff;
				}
			}

			.desc {
				display: inline-block;
				vertical-align: top;
				margin: 12px 0;
				padding: 0 12px;
				color: rgba(255, 255, 255, 0.4);
				font-size: 14px;
				font-weight: 300;
				line-height: 24px;
			}
		}

		.content-right {
			flex: 0 0 105px;
			background: #2b343c;
			box-sizing: border-box;
			width: 105px;
			color: rgba(255, 255, 255, 0.4);
			font-size: 12px;
			font-weight: 700;
			line-height: 46px;
			text-align: center;

			.pay.high-light {
				background: #00cc33;
				color: #fff;
			}
		}

		.ball-container {
			// 一个div负责横向移动，一个div负责纵向移动，配合transition-timing-function可以实现贝塞尔曲线的运动轨迹
			.ball {
				position: fixed;
				left: 32px;
				bottom: 22px;
				z-index: 200;
				transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41);

				.inner {
					width: 16px;
					height: 16px;
					border-radius: 50%;
					background: rgb(0, 160, 220);
					transition: all 0.4s linear;
				}
			}
		}

		.shopcart-list {
			position: fixed;
			width: 100%;
			height: auto;
			max-height: 219.5px;
			bottom: 46px;
			background: #fff;
			overflow: hidden;
			transform: translate3d(0, 0, 0);
			z-index: -1;
			opacity: 1;

			&.translate-enter, &.translate-leave-to {
				transform: translate3d(0, 100%, 0);
				opacity: 0;
			}

			&.translate-enter-active, &.translate-leave-active {
				transition: all 0.4s ease;
			}

			.list-header {
				height: 40px;
				padding: 0 18px;
				background: #f3f5f7;
				display: flex;
				align-items: baseline;
				justify-content: space-between;
				border-bottom: 2px solid rgba(7, 17, 27, 0.1);
				box-sizing: border-box;

				.title {
					display: inline-block;
					font-size: 14px;
					font-weight: 200;
					line-height: 40px;
					color: rgb(7, 17, 27);
				}

				.empty {
					font-size: 12px;
					line-height: 40px;
					color: rgb(0, 160, 220);
				}
			}

			.list-content {
				max-height: 179.5px;
				overflow: hidden;

				.food {
					height: 48px;
					padding: 12px 18px;
					line-height: 48px;
					border-1px(rgba(7, 17, 27, 0.1));

					&:after {
						left: 18px;
						right: 18px;
						width: auto;
					}

					.name {
						font-size: 14px;
						color: rgb(7, 17, 27);
						line-height: 24px;
					}

					.price {
						display: inline-block;

						span {
							display: inline-block;
							font-size: 14px;
							font-weight: 700;
							color: rgb(240, 20, 20);
							line-height: 24px;
						}
					}

					.cart-control {
						float: right;
					}
				}
			}
		}
	}
}
</style>
