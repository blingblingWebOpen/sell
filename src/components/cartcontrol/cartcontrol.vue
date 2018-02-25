<template>
  <div class="cartcontrol">
    <transition name="move">
    <div class="cart-decrease" @click.stop.prevent="decreaseCart" v-show="food.count>0">
        <span class="inner icon-remove_circle_outline"></span>
    </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  // 将在各处使用该事件中心
  // 组件通过它来通信
  var eventHub = new Vue();
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart() {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count ++;
        }
        eventHub.$emit('cart-add', this.target);
        // this.$dispatch('cart.add', event.target);// 首先拿到这个元素  用这个方法$dispatch派发一个事件，把event.target即dom对象传入
      },
      created() {
        eventHub.$on('cart-add', this.target);
      },
      decreaseCart() {
        if (this.food.count) {
          this.food.count --;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease
      display: inline-block
      padding: 6px
      transition: all 0.4s linear
      &.move-transition
        opacity: 1
        transform: translate3d(0,0,0)
      .inner
        display: inline-block
        font-size: 24px
        line-height: 24px
        color: rgb(0, 160, 220)
        transition: all  0.4s linear
        transform: rotate(0)
      &.move-enter, &.move-leave
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147,153,159)
    .cart-add
      display: inline-block
      padding: 6px
      font-size: 24px
      line-height: 24px
      color: rgb(0, 160, 220)

</style>
