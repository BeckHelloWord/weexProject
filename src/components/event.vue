<template>
    <div class="event">
      <text>{{elemnt.name}}</text>
      <div class="btn-wrap">
        <text class="btn-done" @click="onFinish(elemnt,index)" v-if="isFinishBtn">完成</text>
        <text class="iconfont">&#xe62e;</text>
      </div>
    </div>
</template>
<script>true
export default {
    props:["elemnt","index","isFinishBtn"],
  data() {
    return {
    };
  },
  beforeCreate() {
    const domModule = weex.requireModule("dom");
    domModule.addRule("fontFace", {
      fontFamily: "iconfont",
      src: "url('//at.alicdn.com/t/font_913239_v1xlaynood.ttf')"
    });
  },
  methods: {
      onFinish(elemnt,index){
          this.$emit('onFinish',elemnt,index);
      }
  }
};
</script>

<style scoped>
.iconfont {
  font-family: iconfont;
}
.event {
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  padding-left: 20px;
  padding-right: 20px;
  height: 80px;
  background-color: #ffffff;
}

.btn-wrap {
  flex-direction: row;
  align-items: center;
}
.btn-done {
  border: none;
  padding-top: 6px;
  padding-bottom: 6px;
  padding-left: 18px;
  padding-right: 18px;
  border-width: 2px;
  border-style: solid;
  border-color: #999;
  border-radius: 10px;
}
</style>

