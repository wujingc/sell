<template>
  <div class="goods" @click="test">
    <div class="menu-box" id="menubox" ref="menu">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current': currentIndex === index}" @click="selectItem(index,$event)">
          <div class="info">
            <span v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>
            <span class="text">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-box" id="foodbox" ref="food">
      <ul>
        <li class="food-list food-list-hook" v-for="item in goods">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="food-item" v-for="food in item.foods">
              <div class="pic">
                <img :src="food.icon">
              </div>
              <div class="info">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span><span v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="control-box">
                  <carcontrol :food="food"></carcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopCart :deliverFood="deliverFood" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopCart>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import shopCart from '../shopcart/shopcart.vue';
  import carcontrol from '../carcontrol/cartcontrol.vue';
  var errOk = 0;
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data: function() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        a: 0
      };
    },
    computed: {
      currentIndex: function() {
        for (let i = 0; i < this.listHeight.length; i++) {
          var height1 = this.listHeight[i];
          var height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          };
        };
        return 0;
      },
      deliverFood: function() {
        var foods = [];
        // 把每一个food都遍历出来，传到数组里，每个数组里都是一个json对象
        this.goods.forEach(function(good) {
          good.foods.forEach(function(food) {
            if (food.count) {
              foods.push(food);
            };
          });
        });
        return foods;
      }
    },
    created: function() {
      this.classMap = ['decrease', 'discount', 'guarantee', 'invoice', 'special'];
      this.$http.get('api/goods').then(function(res){
        res = res.body;
        if (res.errno === errOk) {
          this.goods = res.data;
          //  data和errno是自定义的属性 在nexttick之后dom就渲染好了
          this.$nextTick(function(){
            this.initBscroll();
            this.calculateHeight();
          });
        }
      });
    },
    methods: {
      initBscroll: function() {
        // var menuBox = document.getElementById('menubox');
        // var foodBox = document.getElementById('foodbox');
        this.menuScroll = new BScroll(this.$refs.menu, {
          click: true
        });
        //  用this.foodScroll则可以全局调用了
        this.foodScroll = new BScroll(this.$refs.food, {
          click: true,
          probeType: 3
        });
        // 此处一定要用箭头函数
        this.foodScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      selectItem: function(index, event){
        //  取消betterscroll的点击事件
        if (!event._constructed) {
          return;
        }
        var foodList = document.getElementsByClassName('food-list-hook');
        var listItem = foodList[index];
        this.foodScroll.scrollToElement(listItem, 300);
      },
      calculateHeight: function() {
        var foodList = document.getElementsByClassName('food-list-hook');
        var height = 0;
        this.listHeight.push(height);
        for (var i = 0; i < foodList.length; i++) {
          height += foodList[i].clientHeight;
          this.listHeight.push(height);
        };
      },
      test: function(){
        this.a += 1;
      }
    },
    components: {
      shopCart,
      carcontrol
    }
  };
  //  default应该是默认导出，对应的是命名导出
</script>

<style>
  .goods{position: absolute; top: 174px; bottom: 46px; width: 100%;}
  .goods .menu-box{float: left; width: 80px; background-color: #f3f5f7;height: 100%; overflow: hidden;}
  .goods .menu-box .menu-item{display: table; width: 56px; height: 54px; line-height: 14px;width: 100%; font-size: 0;}
  .goods .menu-box .current{position: relative; z-index: 10; margin-top: -1px; font-weight: 700; background-color: #fff; }
  .goods .menu-box .current .text{border:none;}
  .goods .menu-box .menu-item .info{display: table-cell;vertical-align: middle;padding: 0 12px; border-bottom: 1px solid rgba(7,17,27,0.1);}
  .goods .menu-box .menu-item .icon{display: inline-block;height: 12px;width: 12px; margin-right: 2px;background-size: 12px 12px;background-repeat: no-repeat;vertical-align: top;}
  .goods .menu-box .menu-item .decrease{background-image: url('img/decrease_3@2x.png');}
  .goods .menu-box .menu-item .discount{background-image: url('img/discount_3@2x.png');}
  .goods .menu-box .menu-item .guarantee{background-image: url('img/guarantee_3@2x.png');}
  .goods .menu-box .menu-item .invoice{background-image: url('img/invoice_3@2x.png');}
  .goods .menu-box .menu-item .special{background-image: url('img/special_3@2x.png');}
  .goods .menu-box .menu-item .text{font-size: 12px;line-height: 12px;vertical-align: top;}
  .goods .foods-box{margin-left: 80px;border-left: 2px solid #d9dde1;overflow: hidden;height: 100%;}
  .foods-box .title{padding-left: 14px; height: 26px; line-height: 26px; font-size: 12px; color: rgb(147,153,159); background-color: #f3f5f7;}
  .foods-box .food-item{position:relative;overflow: hidden; margin: 18px; padding-bottom:18px;margin-bottom:0;border-bottom: 1px solid rgba(7,17,27,0.1);}
  .foods-box .food-item:last-child{border:none;}
  .foods-box .food-item .pic{float: left; width: 57px; margin-right: 10px;}
  .foods-box .food-item .pic img{width: 57px; height: 57px;}
  .foods-box .food-item .info{margin-left: 57px;}
  .foods-box .food-item .info .name{margin: 2px 0 8px 0; height: 14px; line-height: 14px; font-size: 14px; color: rgb(7,17,27);}
  .foods-box .food-item .info .desc{margin-bottom: 8px; line-height: 12px; font-size: 10px; color: rgb(147,153,159);}
  .foods-box .food-item .info .extra{line-height: 10px;font-size:10px;color: rgb(147,153,159);}
  .foods-box .food-item .info .extra .count{margin-right: 12px;}
  .foods-box .food-item .info .price{font-weight: 700; line-height: 24px;}
  .foods-box .food-item .info .price .now{margin-right: 8px; font-size: 14px; color: rgb(240,20,20);}
  .foods-box .food-item .info .price .old{text-decoration: line-through; font-size: 10px; color: rgb(147,153,159);}
  .foods-box .food-item .info .control-box{position: absolute;right: 0;bottom: 12px}
</style>
