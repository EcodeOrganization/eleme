<template>
  <div class="cartControl">
    <transition name="fade">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart($event)">
        <transition name="inner">
          <span class="inner iconfont icon-jian"></span>
        </transition>
      </div>
    </transition>
    <span class="cart-count" v-show="food.count > 0 ">
      {{food.count}}
    </span>
    <span class="iconfont icon-jia cart-add" @click.stop.prevent="addCart($event)"></span>
  </div>
</template>

<script type="text/ecmascript-6">
import Vue from "vue";
export default {
  props: {
    food: {
      type: Object,
      default: () => {
        return {};
      }
    }
  },
  data() {
    return {
      showMoveDot: [] // 控制下落的小圆点显示隐藏
    };
  },
  methods: {
    addCart(event) {
      if (!event._constructed) {
        // 去掉自带click事件的点击
        return;
      }
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
      let elLeft = event.target.getBoundingClientRect().left;
      let elBottom = event.target.getBoundingClientRect().bottom;
      this.showMoveDot = [true];
      this.$emit("showMoveDot", this.showMoveDot, elLeft, elBottom);
    },
    decreaseCart(event) {
      if (!event._constructed) {
        // 去掉自带click事件的点击
        return;
      }
      this.food.count--;
    }
  }
};
</script>
<style lang="stylus" scoped>
.cartControl {
  font-size: 0;

  .cart-decrease, .cart-add {
    display: inline-block;
    padding: 0.04rem 0.06rem 0.06rem 0.06rem;

    &.fade-enter-active, &.fade-leave-active {
      transition: all 0.4s linear;
    }

    &.fade-enter, &.fade-leave-active {
      opacity: 0;
      transform: translate3d(0.24rem, 0, 0);
    }

    .inner {
      display: inline-block;
      line-height: 0.24rem;
      font-size: 0.24rem;
      vertical-align: top;
      color: rgb(0, 160, 220, 0.2);

      &.inner-enter-active, &.inner-leave-active {
        transition: all 0.4s linear;
        transform: rotate(0);
      }

      &.inner-enter, &.inner-leave-active {
        opacity: 0;
        transform: rotate(180deg);
      }
    }
  }

  .cart-count {
    display: inline-block;
    font-size: 0.1rem;
    line-height: 0.24rem;
    width: 0.12rem;
    vertical-align: top;
    padding-top: 0.06rem;
    text-align: center;
    color: rgb(147, 153, 159);
  }

  .cart-add {
    display: inline-block;
    padding: 0.06rem;
    line-height: 0.24rem;
    font-size: 0.24rem;
    vertical-align: top;
    color: rgb(0, 160, 220, 0.2);
  }
}
</style>
