<template>
  <div id="app">
    <v-header v-bind:seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item"><router-link to="/goods">商品</router-link></div>
      <div class="tab-item"><router-link to="/ratings">评论</router-link></div>
      <div class="tab-item"><router-link to="/seller">商家</router-link></div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import header from './components/header/header.vue';

  var errOk = 0;
  export default{
    //  初始化seller数据为空对象
    data: function(){
      return {
        seller: {}
      };
    },
    created: function(){
      this.$http.get('api/seller').then(function(res){
        res = res.body;
        if (res.errno === errOk){
          this.seller = res.data;
        }
      });
    },
    components: {
      'v-header': header
    }
  };
</script>

<style>
  body{line-height: 1;font-weight: 200;font-family: 'PingFang SC','STHeitiSC-Light',arial,'sans-serif'}
  .clearfix{display: inline-block;}
  .clearfix:after{display: block; content: '.'; height: 0; line-height: 0; clear: both; visibility: hidden;}
/*  .clearfix:before, .clearfix:after{
    content: "";
    display: table;
  }
  .clearfix:after{clear: both;}*/

    #app{width: 100%;overflow: hidden;}
  #app .tab{position:relative;width: 100%;height: 40px;line-height: 40px;}
  #app .tab:after{display: block; width:100%; position: absolute;left: 0;bottom: 0;border-top:1px solid rgba(7,17,27,0.1); content: '';}
  @meida (-webkit-min-device-pixel-ratio:1.5),(min-device-pixel-ratio:1.5){
    #app .tab:after{-webkit-transform: scaleY(0.7);
            transform: scaleY(0.7);}
  }
  @meida (-webkit-min-device-pixel-ratio:2),(min-device-pixel-ratio:2){
    #app .tab:after{-webkit-transform: scaleY(0.5);
            transform: scaleY(0.5);}
  }
  #app .tab .tab-item{width: 33.333%;white-space: nowrap;float: left;text-align: center;}
  #app .tab .tab-item a{display: block;font-size: 14px;color:rgb(77,85,93);}
  #app .tab .tab-item .router-link-active{color:rgb(240,20,20);}
</style>
