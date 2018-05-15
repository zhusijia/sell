<template>
	<transition name="move">
		<div v-show="showFlag" class="food">
			<div class="food-content">
				<div class="image-header">
					<img :src="food.image">
					<div class="back" @click="hide">
						<i class="ion icon-arrow_lift"></i>
					</div>
				</div>
			</div>
		</div>
	</transition>
</template>

<script>
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
  methods: {
    show() {
      this.showFlag = true;
    },
		hide() {
			this.showFlag = false;
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
	background: #fff;

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
}
</style>
