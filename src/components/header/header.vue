<template>
  <div class="header">
    <div class="header-avatar">
      <div class="pic">
        <img v-bind:src="seller.avatar" width="64px" height="64px">
      </div>

      <div class="info">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support" v-if="seller.supports">
          <span class="icon" v-bind:class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>

    <div class="header-bulletin" v-on:click="showDetail">
      <span class="title"></span><span class="text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>

    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <transition name="fade">
    <div class="detail" v-if="detailShow">
      <div class="detail-box clearfix">
        <div class="detail-main">
          <h1 class="name">
            {{seller.name}}
          </h1>
          <div class="star-wrapper">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="detail-title clearfix">
            <div class="line"></div>
            <div class="text">商家信息</div>
            <div class="line"></div>
          </div>
          <div class="detail-list">
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item, index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
          </div>
          <div class="detail-title clearfix">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p>{{seller.bulletin}}</p>
          </div>
        </div>

      </div>
      <div class="detail-close" @click="closeDetail">
        <i class="icon-close"></i>
      </div>
    </div>
    </transition>
  </div>
</template>

<script>
  import star from '../star/star.vue';
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    //  data是动态跟踪的
    data: function(){
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail(){
        this.detailShow = true;
      },
      closeDetail(){
        this.detailShow = false;
      }
    },
    created: function(){
      //  静态的数据
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
    },
    components: {
      star
    }
  };
  //  default应该是默认全部输出，没有指定输出哪一个对象
</script>

<style>
  .header{position: relative;color: #fff;width: 100%;background-color: rgba(7,17,27,0.5);overflow: hidden;}
  .header .header-avatar{padding: 24px 12px 18px 24px;font-size: 0;position: relative;}
  .header .header-avatar .pic{display: inline-block;vertical-align: top;}
  .header .header-avatar .pic img{border-radius: 2px;}
  .header .header-avatar .info{display: inline-block;font-size: 14px;margin-left: 16px;}
  .header .header-avatar .info .title{margin: 2px 0 8px 0;font-size: 0px;}
  .header .header-avatar .info .title .brand{width: 30px;height: 18px; display: inline-block;background: url(img/brand@2x.png) no-repeat;background-size: 30px 18px;vertical-align: top;}

  .header .header-avatar .info .title .name{font-size: 16px;font-weight: bold;margin-left: 6px; line-height: 18px;}
  .header .header-avatar .info .description{font-size: 12px;line-height: 12px;margin-bottom: 10px;}

  .header .header-avatar .info .support{font-size: 0px;}
  .header .header-avatar .info .support .icon{display: inline-block;height: 12px;width: 12px; margin-right: 4px;background-size: 12px 12px;background-repeat: no-repeat;vertical-align: top;}
  .header .header-avatar .info .support .decrease{background-image: url('img/decrease_1@2x.png');}
  .header .header-avatar .info .support .discount{background-image: url('img/discount_1@2x.png');}
  .header .header-avatar .info .support .guarantee{background-image: url('img/guarantee_1@2x.png');}
  .header .header-avatar .info .support .invoice{background-image: url('img/invoice_1@2x.png');}
  .header .header-avatar .info .support .special{background-image: url('img/special_1@2x.png');}
  .header .header-avatar .info .support .text{font-size: 10px;line-height: 12px;}
  @media (-webkit-min-device-pixel-ratio:3),(min-device-pixel-ratio:3){
    .header .header-avatar .info .title .brand{background: url(img/brand@3x.png) no-repeat;background-size: 30px 18px;}
    .header .header-avatar .info .support .decrease{background-image: url('img/decrease_1@3x.png');}
    .header .header-avatar .info .support .discount{background-image: url('img/discount_1@3x.png');}
    .header .header-avatar .info .support .guarantee{background-image: url('img/guarantee_1@3x.png');}
    .header .header-avatar .info .support .invoice{background-image: url('img/invoice_1@3x.png');}
    .header .header-avatar .info .support .special{background-image: url('img/special_1@3x.png');}
  }
  .header .header-avatar .support-count{position: absolute;right: 12px;bottom: 18px;padding: 0 8px;height: 24px;line-height: 24px;border-radius: 14px;background-color: rgba(0,0,0,0.2);text-align: center;font-size: 0;}
  .header .header-avatar .support-count span{vertical-align: top;font-size: 10px;}
  .header .header-avatar .support-count i{line-height: 24px;margin-left: 2px;font-size: 10px;}

  .header .header-bulletin{position: relative; height: 28px;line-height: 28px;padding: 0 22px 0 12px;white-space: nowrap;overflow: hidden;text-overflow: ellipsis;background-color: rgba(7,17,27,0.2);}
  .header .header-bulletin .title{vertical-align:top;display: inline-block; width: 22px; height: 12px; margin-top: 8px; background-image: url(img/bulletin@2x.png);background-size: 22px 12px;background-repeat: no-repeat;}
  .header .header-bulletin .text{vertical-align:top;font-size: 10px;margin: 0 4px;}
  .header .header-bulletin i{position: absolute;right:12px;top:8px;font-size: 10px;}
  .header .background{position: absolute; z-index: -1;top: 0;left:0;width: 100%;height: 100%;filter:blur(10px);}

  .header .detail{position: fixed; z-index: 100; top: 0; left:0; width: 100%; height: 100%; overflow: auto;background-color: rgba(7,17,27,0.8); backdrop-filter: blur(10px);}
  .header .fade-enter-active, .header .fade-leave-active{transition: all 0.5s;}
  .header .fade-enter, .header .fade-leave-active{opacity: 0;}
  .header .detail .detail-box{min-height: 100%; width: 100%;}
  .header .detail .detail-box .detail-main{margin-top: 64px; padding-bottom: 64px;}
  .header .detail .detail-box .detail-main .name{line-height: 16px; text-align: center; font-size: 16px; font-weight: 700;}
  .header .detail .detail-box .detail-main .star-wrapper{margin-top: 18px;padding: 2px 0;text-align: center;}
  /*第一种方法*/
/*  .header .detail .detail-box .detail-main .detail-title{width:80%;margin: 30px auto 24px auto;text-align: center;}
  .header .detail .detail-box .detail-main .detail-title .line{position: relative;border-top: 1px solid rgba(255,255,255,0.2); top: 6px;width: calc((100% - 104px) /2);float: left;}
  .header .detail .detail-box .detail-main .detail-title .text{float: left;font-size: 14px;text-align: center;padding: 0 12px;}*/
  /*flex方法*/
  .header .detail .detail-box .detail-main .detail-title{display:flex;width:80%;margin: 28px auto 24px auto;}
  .header .detail .detail-box .detail-main .detail-title .line{flex:1;position: relative;border-top: 1px solid rgba(255,255,255,0.2); top: 6px;}
  .header .detail .detail-box .detail-main .detail-title .text{font-size: 14px;text-align: center;padding: 0 12px;font-weight: 700;}
  .header .detail .detail-close{position:relative; width: 32px; height: 32px; margin: 0 auto; clear: both; font-size: 32px;margin-top: -64px;}

  .header .detail-list .supports{font-size: 0px;width: 80%; margin: 0 auto;}
  .header .detail-list .supports .support-item{margin-bottom: 12px; padding: 0 12px;}
  .header .detail-list .supports .support-item:last-child{margin-bottom: 0;}
  .header .detail-list .supports .icon{display: inline-block;height: 16px;width: 16px; margin-right: 6px;background-size: 16px 16px;background-repeat: no-repeat;vertical-align: top;}
  .header .detail-list .supports .decrease{background-image: url('img/decrease_2@2x.png');}
  .header .detail-list .supports .discount{background-image: url('img/discount_2@2x.png');}
  .header .detail-list .supports .guarantee{background-image: url('img/guarantee_2@2x.png');}
  .header .detail-list .supports .invoice{background-image: url('img/invoice_2@2x.png');}
  .header .detail-list .supports .special{background-image: url('img/special_2@2x.png');}
  .header .detail-list .supports .text{font-size: 12px;line-height: 16px;}
  .header .detail .bulletin{width: 80%; margin: 0 auto;padding: 0 12px;}
  .header .detail .bulletin p{line-height: 24px;font-size: 12px;}
</style>
