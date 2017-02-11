<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="decrease" v-show="food.count>0" @click="decCount">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
      <div class="count" v-show="food.count>0">{{food.count}}</div>
      <div class="add icon-add_circle" @click="addCount"></div>
  </div>
</template>

<script>
  import Vue from 'vue';
  export default{
    props: {
      food: {
        type: Object
      }
    },
    //  计算属性自动会执行
    computed: {

    },
    created: function() {
    },
    methods: {
      addCount: function(event) {
        if (!event._constructed) {
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
          //  此处才初始化了count的值
        } else {
          this.food.count ++;
        }
      },
      decCount: function(event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count --;
        }
      }
    }
  };
</script>

<style>
  .cartcontrol{font-size: 0;}
  .cartcontrol .decrease{display: inline-block; padding: 6px;opacity: 1;transform: translate3d(0, 0, 0);}
  /*不要这两个状态，直接写在元素上也可以*/
  .cartcontrol .move-enter-active, .cartcontrol .move-leave-active{
    transition: all .4s linear;
  }
  .cartcontrol .move-enter-active .inner, .cartcontrol .move-leave-active .inner{
    transition: all .4s linear;
  }
  /*开始瞬间状态*/
  .cartcontrol .move-enter, .cartcontrol .move-leave-active{
    opacity: 0;
    transform: translate3d(24px, 0, 0);
  }
  .cartcontrol .move-enter .inner, .cartcontrol .move-leave-active .inner{
     transform: rotate(180deg);
  }
  .cartcontrol .inner{display:inline-block;font-size: 24px; line-height: 24px; color: rgb(0, 160, 220);transform: rotate(0);transition: all .4s linear;}
  .cartcontrol .count{display: inline-block;vertical-align: top;width: 12px;padding-top: 6px;line-height: 24px;text-align: center;font-size: 10px;color: rgb(147, 153, 159)}
  .cartcontrol .add{display: inline-block; font-size: 24px; line-height: 24px;color: rgb(0, 160, 220);padding: 6px;}
</style>
