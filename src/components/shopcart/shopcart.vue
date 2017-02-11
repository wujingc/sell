<template>
    <div class="shopcart">
      <div class="shopcart-box clearfix">
        <div class="left">
          <div class="logo-box">
            <div class="logo" :class="{'highlight': totalCount>0}">
              <i class="icon-shopping_cart" :class="{'iconhigh': totalCount>0}"></i>
            </div>
            <div class="num" v-show="totalCount>0">{{totalCount}}</div>
          </div>
          <div class="price" :class="{'texthigh': totalCount>0}">￥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <div class="right">
          <div class="pay" :class="{'payhigh': this.totalPrice >= this.minPrice}">
            {{paysRight}}
          </div>
        </div>
      </div>
      <div class="ball-box">
        <div v-for="ball in balls">
          <transition>
            <div class="ball" v-show="ball.show">
              <div class="inner"></div>
            </div>
          </transition>
        </div>
      </div>
    </div>
</template>

<script>
  export default{
    props: {
      deliverFood: {
        type: Array,
        //  对象和数组的时候要用函数返回
        default: function() {
          return [
            {
              price: 1,
              count: 5
            }
          ];
        }
      },
      deliveryPrice: {
        type: Number
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data: function() {
      return {
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
        ]
      };
    },
    computed: {
      totalPrice: function() {
        var total = 0;
        this.deliverFood.forEach(function(food) {
          total = food.price * food.count;
        });
        return total;
      },
      totalCount: function() {
        var totalCount = 0;
        this.deliverFood.forEach(function(food) {
          totalCount += food.count;
        });
        return totalCount;
      },
      paysRight: function() {
        if (this.totalPrice === 0) {
          return '￥' + this.minPrice + '起送';
        } else if (this.totalPrice < this.minPrice) {
          var diff = this.minPrice - this.totalPrice;
          return '还差￥' + diff + '元起送';
        } else {
          return '去结算';
        };
      }
    },
    methods: {
    }
  };
</script>

<style>
  .shopcart{position: fixed;left: 0;bottom: 0;z-index: 50;width: 100%;height: 48px;}
  .shopcart .shopcart-box{width:100%;background: #141d27;font-size: 10px;color: rgba(255, 255, 255, 0.4)}
  .shopcart .shopcart-box .left{float: left;}
  .shopcart .shopcart-box .left .logo-box{display: inline-block;vertical-align: top;position: relative;top: -10px;margin: 0 12px;padding: 6px;width: 56px;height: 56px;box-sizing: border-box;border-radius: 50%;background: #141d27}
  .shopcart .shopcart-box .left .logo-box .logo{width: 100%;height: 100%;border-radius: 50%;text-align: center;background: #2b343c;}
   .shopcart .shopcart-box .left .logo-box .highlight{background: rgb(0, 160, 220);}
   .shopcart .shopcart-box .left .logo-box .logo .iconhigh{color: #fff;}
  .shopcart .shopcart-box .left .num{position: absolute;top: 0;right: 0;width: 24px;height: 16px;line-height: 16px;text-align: center;border-radius: 16px;font-size: 9px;font-weight: 700;color: #fff;background: rgb(240, 20, 20);box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4)}
  .shopcart .shopcart-box .left .price{display: inline-block;vertical-align: top;margin-top: 12px;line-height: 24px;padding-right: 12px;box-sizing: border-box;border-right: 1px solid rgba(255, 255, 255, 0.1);font-size: 16px;font-weight: 700}
  .shopcart .shopcart-box .left .texthigh{color:#fff;}
  .shopcart .shopcart-box .left .desc{display: inline-block;vertical-align: top;margin: 12px 0 0 12px;line-height: 24px;font-size: 10px}
  .shopcart .shopcart-box .left .logo-box .logo i{ font-size: 24px;color: #80858a;line-height: 44px;}
  .shopcart .shopcart-box .right{float: right;}
  .shopcart .shopcart-box .right .pay{width:105px;height: 48px; line-height: 48px; text-align: center; font-size: 12px; font-weight: 700;background-color: #2b333b;}
  .shopcart .shopcart-box .right .payhigh{background: #00b43c;color: #fff}

  .shopcart .ball-box .ball{position: fixed;left: 32px;bottom: 22px;z-index: 200;transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)}
  .shopcart .ball-box .inner{width: 16px;height: 16px;border-radius: 50%;background: rgb(0, 160, 220);transition: all 0.4s linear}
</style>
