<template>
	<div class="shop-cart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
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
				<div class="pay" :class="{'high-light': totalPrice >= minPrice}">
					<span v-if="totalPrice === 0">¥{{minPrice}}元起送</span>
					<span v-if="deliverPrice > 0">还差¥{{deliverPrice}}起送</span>
					<span v-else>去结算</span>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
const ERR_OK = 0;
export default {
  name: "shopCarts",
  data() {
    return {
      seller: {}
    };
  },
  created() {
    this.$http.get("/api/seller").then(response => {
      response = response.body;
      if (response.errno === ERR_OK) {
        this.seller = response.data;
      }
    });
  },
  props: {
    selectFoods: {
      type: Array,
      default() {
        return [{ price: 20, count: 1 }];
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
		}
  },
  methods: {}
};
</script>
<style lang="stylus" scoped>
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
	}
}
</style>
