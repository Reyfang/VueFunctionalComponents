<template>
    <div class="redPackBall" :class="{'redPackAni':isShowAni}" ref="redPackBall"
    @touchstart="start"
    @touchmove="move"
    @touchend="end">
        <span>懸浮球</span>
    </div>
</template>


<script>
export default {
  name: "redPack",
  data() {
    return {
      isShowAni: false,
      _x_start: 0,
      _y_start: 0,
      _x_move: 0,
      _y_move: 0,
      left_start: 0,
      top_start: 0
    };
  },
  methods: {
    start(e) {
      this.isShowAni = false;
      this._x_start = e.touches[0].pageX; //起始点击位置
      this._y_start = e.touches[0].pageY; //起始点击位置
      let moveDiv = this.$refs.redPackBall;
      this.left_start = moveDiv.offsetLeft; //元素左边距
      this.top_start = moveDiv.offsetTop; //元素上边距
    },
    move(e) {
      e.preventDefault(); //取消事件的默认动作。
      this._x_move = e.touches[0].pageX; //当前屏幕上所有触摸点的集合列表
      this._y_move = e.touches[0].pageY; //当前屏幕上所有触摸点的集合列表
      //左边距=当前触摸点-鼠标起始点击位置+起始左边距
      let moveDiv = this.$refs.redPackBall;
      moveDiv.style.left =
        parseFloat(this._x_move) -
        parseFloat(this._x_start) +
        parseFloat(this.left_start) +
        "px";

      //上边距=当前触摸点-鼠标起始点击位置+起始上边距
      moveDiv.style.top =
        parseFloat(this._y_move) -
        parseFloat(this._y_start) +
        parseFloat(this.top_start) +
        "px";
    },
    end(e) {
      this.isShowAni = true;
      var bodyW = document.body.offsetWidth / 2; //屏幕宽的一半
      var bodyH = document.body.offsetHeight; //屏幕高
      var _x_end = e.changedTouches[0].pageX; //松开位置
      var _y_end = e.changedTouches[0].pageY; //松开位置
      let moveDiv = this.$refs.redPackBall;
      var divH = moveDiv.clientHeight; //元素高
      var divW = moveDiv.clientHeight; //元素宽
      //当最终位置在屏幕左半部分
      if (_x_end < bodyW) {
        moveDiv.style.left = "0px";
      } else if (_x_end >= bodyW) {
        //当最终位置在屏幕左半部分
        moveDiv.style.left =
          document.body.offsetWidth - parseFloat(divW) + "px";
      }
      //当元素顶部在屏幕外时
      if (parseFloat(moveDiv.style.top) < 0) {
        //置于顶部
        moveDiv.style.top = "0px";
      }
      //当元素底部在屏幕外时
      if (bodyH - _y_end < parseFloat(divH) / 2) {
        //置于底部
        moveDiv.style.top = bodyH - parseFloat(divH) + "px";
      }
    }
  }
};
</script>


<style lang="scss" scoped>
.redPackBall {
  z-index: 99;
  width: 45px;
  height: 45px;
  text-align: center;
  position: fixed;
  left: 0px;
  top: 100px;
  font-size: 16px;
  border-radius: 50%;
  background-color: rgba(0, 0, 0, 0.5);
  font-size: 16px;
  color: #fff;
  box-shadow: 0 0 15px 1px #fff;
}
.redPackAni {
  transition: all 0.5s;
}
</style>
