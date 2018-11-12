<template>
  <div class="wrapper" @viewappear="viewappear">
    <text class="title ">待办事项</text>
    <event-todo v-for="(elemnt,index) in todoEvent" :key="elemnt.name" :elemnt="elemnt" :index="index" @onFinish="onFinish" isFinishBtn="true"></event-todo>
    <text class="title">已办事项</text>
    <event-todo v-for="elemnt in finishEvent" :key="elemnt.name" :elemnt="elemnt" :index="index" ></event-todo>
    <div class="btns-wrap">
      <div class="btns">
         <text class="iconfont">&#xe889;</text>
         <text class="b-btn" @click="toAdd">添加</text>
      </div>
      <div class="btns">
        <text class="iconfont">&#xe8c1;</text>
        <text class="b-btn" @click="clearData">清空</text>
      </div>
    </div>
  </div>
</template>

<script>
const storage = weex.requireModule("storage");
const navigator = weex.requireModule("navigator");
import eventTodo from './components/event.vue';
export default {
  name: "App",
  components: {eventTodo},
  data() {
    return {
      todoEvent: [],
      finishEvent: []
    };
  },
  methods: {
    onFinish(event, index) {
      this.todoEvent.splice(index, 1);
      this.finishEvent.push(event);
      this.stroeData();
    },
    stroeData() {
      storage.setItem("todoEvent", JSON.stringify(this.todoEvent));
      storage.setItem("finishEvent", JSON.stringify(this.finishEvent));
    },
    toAdd() {
      navigator.push({
        url: "./add.html",
        animated: "true"
      });
    },
    viewappear() {
      storage.getItem("todoEvent", e => {
        if (e.result === "success") {
          this.todoEvent = JSON.parse(e.data) || [];
        }
      });

    	storage.getItem("finishEvent", e => {
        if (e.result === "success") {
          this.finishEvent = JSON.parse(e.data) || [];
        }
      });
    },
    clearData() {
      this.todoEvent = [];
      this.finishEvent = [];
      this.stroeData();
    }
  },
  beforeCreate() {
    const domModule = weex.requireModule("dom");
    domModule.addRule("fontFace", {
      fontFamily: "iconfont",
      src: "url('//at.alicdn.com/t/font_913239_v1xlaynood.ttf')"
    });
  },
  created() {
    if (weex.config.env.platform === "Web") {
      this.viewappear();
    }
  }
};
</script>

<style scoped>
.wrapper {
  background-color: #f0f0f2;
}
.iconfont {
  font-size:56px;
  font-family: iconfont;
}
.title {
  padding-top: 20px;
  padding-bottom: 20px;
  padding-left: 10px;
  padding-right: 10px;
  font-size: 32px;
}



.btns-wrap {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 750px;
  height: 100px;
  align-items: center;
  flex-direction: row;
  background-color: #fff;
}
.btns{
  flex: 1;
  flex-direction:row;
  justify-content:center;
  align-items:center;
}
.b-btn {
  text-align: center;
}
</style>
