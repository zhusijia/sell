<template>
	<div class="cart-control">
		<transition name="transition">
			<div class="">
				<div class="cart-decrease icon-remove_circle_outline" v-show="food.count > 0" @click="decreaseCart"></div>
			<div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
			</div>
		</transition>
		<!-- <div class="cart-decrease icon-remove_circle_outline" v-show="food.count > 0" @click="decreaseCart"></div>
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
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
    },
    decreaseCart() {
      if (!this.food.count) {
        return;
      } else {
        this.food.count--;
      }
    }
  }
};
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/mixin';

.cart-control {
	font-size: 0;
	display: flex;
	align-items: center;

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

	.cart-cart-add {
		display: inline-block;
		padding: 6px;
		font-size: 24px;
		line-height: 24px;
		color: rgb(0, 160, 220);
	}
}
</style>

