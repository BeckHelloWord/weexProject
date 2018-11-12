<template>
    <div class="wrapper">
        <div class="header">
            <text class="iconfont" @click="goBack">&#xe62f;</text>
            <text class="title">添加事件</text>
        </div>
        <div class="form-items">
            <text class="intpu-title">事件标题</text>
            <input type="text" class="input-content" v-model="name"/>
        </div>
        <div class="form-items">
            <text class="intpu-title">事件详情</text>
            <textarea class="textarea" placeholder="请输入事件详情" v-model="desc"></textarea>
        </div>
        <div class="btn-add" @click="addEvent">
            <text>完成</text>
        </div>
    </div>  
</template>
<script>
const storage = weex.requireModule("storage");
const navigator = weex.requireModule("navigator");
export default {
  name: "add",
  data() {
    return {
      name: "",
      desc: ""
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
    addEvent() {
      if (!this.name || !this.desc) {
        return false;
      }
      let todoEvent = [];
      storage.getItem("todoEvent", e => {
        if (e.result === "success") {
          todoEvent = JSON.parse(e.data);
          todoEvent.push({ name: this.name, desc: this.desc });
        }
      });

      storage.setItem("todoEvent", JSON.stringify(todoEvent), e => {
        if (e.result === "success") {
          navigator.pop({ animated: "true" });
        }
      });
    },
    goBack() {
      navigator.pop({ animated: "true" });
    }
  }
};
</script>

<style scoped>
.iconfont {
  font-family: iconfont;
}
.wrapper {
  flex-direction: column;

  background-color: #f0f0f2;
}
.form-items {
  flex-direction: row;
  align-items: center;
  padding-top: 20px;
  padding-left: 20px;
  padding-right: 20px;
}
.input-content {
  flex: 1;
  height:80px;
  line-height:80px;
  font-size:60px;
}
.textarea {
  flex: 1;
}
.btn-add {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 750px;
  height: 100px;
  justify-content: center;
  align-items: center;
  background-color: #fff;
}
.header {
  flex-direction: row;
  align-items: center;
  height: 100px;
  background-color: #fff;
}
.title {
  flex: 1;
  text-align: center;
}
</style>

