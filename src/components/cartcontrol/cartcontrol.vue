<template>
	<div class="cart-control">
		<transition name="translate">
			<div class="decrease-container" v-show="food.count > 0">
				<div class="cart-decrease" @click="decreaseCart">
					<div class="inner icon-remove_circle_outline"></div>
				</div>
				<div class="cart-count">{{food.count}}</div>
			</div>
		</transition>
		<div class="cart-count" v-show="food.count > 0">{{food.count}}</div> -->
		<div class="cart-cart-add icon-add_circle" @click="addCart"></div>
	</div>
</template>

<script>
import Vue from "vue";
export default {
  props: {
    food: {
      type: Object,
      default: {}
    }
  },
  methods: {
    addCart() {
			event.stopPropagation()
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
			this.$emit('cart-add', event.target)
    },
    decreaseCart() {
      if (!this.food.count) {
        return;
      } else {
        this.food.count--;
      }
    },
  }
};
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/mixin';

.cart-control {
	font-size: 0;
	display: flex;
	align-items: center;

	.decrease-container {
		display: flex;
		align-items: center;
		opacity: 1;
		transform: translate3d(0, 0, 0);

		.inner {
			display: inline-block;
			transform: rotate(0);
		}

		&.translate-enter, &.translate-leave-to {
			transform: translate3d(36px, 0, 0);
			opacity: 0;

			.inner {
				transform: rotate(180deg);
			}
		}

		&.translate-enter-active, &.translate-leave-active {
			transition: all 0.2s linear;

			.inner {
				transition: all 0.2s linear;
			}
		}

		.cart-decrease {
			display: inline-block;
			padding: 6px;
			font-size: 24px;
			line-height: 24px;
			color: rgb(0, 160, 220);
		}

		.cart-count {
			display: inline-block;
			font-size: 10px;
			color: rgb(147, 153, 159);
			line-height: 24px;
			width: 24px;
			text-align: center;
		}
	}

	.cart-cart-add {
		display: inline-block;
		padding: 6px;
		font-size: 24px;
		line-height: 24px;
		color: rgb(0, 160, 220);
	}
}
</style>

