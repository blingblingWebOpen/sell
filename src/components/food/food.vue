<template>
  <transition name="move">
  <div v-show="showFlag" class="food" ref="food">
    <div class="food-content">
      <div class="image-header">
        <img :src="food.image"/>
        <div class="back" @click="hide">
          <i class="icon-arrow_lift"></i>
        </div>
      </div>
      <div class="content">
        <h1 class="title">{{food.name}}</h1>
        <div class="detail">
          <span class="sell-count">月售:{{food.sellCount}}份</span>
          <span class="rating">好评率{{food.rating}}</span>
        </div>
        <div class="price">
          <span class="now">￥{{food.price}}</span>
          <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
        </div>
        <div class="cartcontrol-wrapper">
          <cartcontrol :food="food"></cartcontrol>
        </div>
        <transition name="fade">
          <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count===0">加入购物车</div>
        </transition>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title">商品信息</h1>
        <p class="text">{{food.info}}</p>
      </div>
      <split></split>
      <div class="rating">
        <h1 class="title">商家评论</h1>
        <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
      </div>
    </div>
  </div>
  </transition>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import vue from 'vue';
  import cartcontrol from '../../components/cartcontrol/cartcontrol';
  import split from '../../components/split/split';
  import ratingselect from '../ratingselect/ratingselect';
  // const POSITIVE = 0; // 正向
  // const NEGATIVE = 1; // 负面
  const ALL = 2;      // 全部
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data() {
      return {
        showFlag: false,
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    methods: {
      show() {
        // 父组件能调用子组件方法，反之不行
        this.showFlag = true;
        this.selectType = 1;
        this.onlyContent = true;
        // 当页面被展示的时候，我们加载bscroll
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();// 重新计算
          }
        });
      },
      hide() {
        this.showFlag = false;
      },
      addFirst() {
        // 第一次，我们通过set把count设置为1;
        vue.set(this.food, 'count', 1);
      }
    },
    components: {
      ratingselect,
      cartcontrol,
      split
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .move-enter-active,.move-leave-active
    transition: all 0.3s linear
    transform: translate3d(0,0,0)
  .move-enter,.move-leave-to
    transform: translate3d(100%,0,0)
  .food
    position: fixed
    left: 0
    top: 0
    bottom: 48px
    z-index: 30
    width: 100%
    background: #ffffff
    .image-header
      position: relative
      width: 100%
      height: 0
      padding-top: 100%  // w3c标准提到过，当我们给padding-top/padding-bottom设置为100%的时候，这个值的计算是相对于盒子的宽度去计算的百分比，也就是宽度的100%，也就是上下的宽度相等的，也就是现在盒子就是一个宽高相等的。
      img
        position: absolute
        top: 0
        left: 0
        width: 100%
        height: 100%
      .back
        position: absolute
        top: 10px
        left: 0px
        .icon-arrow_lift
          display: block
          padding: 10px
          font-size: 20px
          color: #fff
    .content
      position: relative
      padding: 18px
      .title
        font-size: 14px
        font-weight: 700
        margin-bottom: 8px
        line-height: 14px
        color: rgb(7,17,27)
      .detail
        margin-bottom: 18px
        font-size: 0
        height: 10px
        line-height: 10px
        color: rgb(147,153,159)
        .sell-count,.rating
          font-size: 10px
        .sell-count
          margin-right: 12px
      .price
        font-weight: 700
        line-height: 24px
        .now
          font-size: 14px
          color: rgb(240,20,20)
        .old
          font-size: 10px
          color: rgb(147,153,159)
    .cartcontrol-wrapper
      position: absolute
      right: 12px
      bottom: 12px
    .buy
      position: absolute
      right: 18px
      bottom: 18px
      z-index: 10
      height: 24px
      line-height: 24px
      padding: 0 12px
      font-size:10px
      color: rgb(255,255,255)
      border-radius: 24px
      background-color: rgb(1,160,220)
    .fade-enter-active,.fade-leave-active
      transition: all 0.3s
      opacity: 1
    .fade-enter, .fade-leave-to
      opacity: 0
    .info
      padding: 18px
      background: #ffffff
      .title
        font-size: 14px
        color: rgb(7,17,27)
        font-weight: 700
        margin-bottom: 12px
      .text
        font-size: 12px
        font-weight: 200
        color: rgb(77,85,93)
        line-height: 24px
        padding: 0 8px
    .rating
      padding-top: 18px
      .title
        font-size: 14px
        color: rgb(7,17,27)
        font-weight: 700
        margin-left: 18px

</style>
