<template>
  <div class="rooms">
    <div class="room" v-for="(r, index) in rooms" :key="index" @click="roomClick">
      <div class="title">
        {{ index == 0 ? "语音识别模块" : "姿态检测模块" }}
      </div>
      <div class="boxs">
        <div class="box" v-for="(item, i) in r" :key="i">
          <div class="name">{{ item.name }}</div>
          <div class="content">{{index==0?'需求':'姿态'}}：{{ item.content }}</div>
        </div>
        <div class="newBox"><span style="font-size: 30px;">+</span><span> 添加房间</span></div>
      </div>
    </div>
    <div class="handles">

    </div>
  </div>
</template>
<script>
import mqtt from "mqtt";
export default {
  name: "indexVue",
  data() {
    return {
      client: {},
      rooms: [
        [
          {
            name: "客厅",
            id: 1,
            flag: 0, // 0绿色，1蓝色，2红色，3灰色
          },
          {
            name: "厨房",
            id: 1,
            flag: 0, // 0绿色，1蓝色，2红色，3灰色
          },
          {
            name: "浴室",
            id: 1,
            flag: 0, // 0绿色，1蓝色，2红色，3灰色
          },
        ],
        [
          {
            name: "厨房",
            id: 1,
            flag: 0, // 0绿色，1蓝色，2红色，3灰色
          },
        ],
      ],
    };
  },
  mounted() {
    this.connect();
  },
  methods: {
    connect() {
      let options = {
        username: "",
        password: "",
        // cleanSession: false,
        // keepAlive: 60,
        // clientId: "mqttjs_" + Math.random().toString(16).substr(2, 8),
        connectTimeout: 4000,
        // port: 8083,
        port: 8084, // wss
      };
      // this.client = mqtt.connect("ws://nuc.scut.mcurobot.com/mqtt", options);
      this.client = mqtt.connect("wss://nuc.scut.mcurobot.com/mqtt", options);
      this.client.on("connect", (e) => {
        console.log("成功连接服务器:", e);
        //订阅三个名叫'top/#', 'three/#'和'#'的主题
        this.client.subscribe(
          ["room1_sr", "room11_sr", "room111_sr"],
          { qos: 1 },
          (err) => {
            if (!err) {
              console.log("订阅成功");
            } else {
              console.log("消息订阅失败！");
            }
          }
        );
      });
      //   //重新连接
      //   this.reconnect();
      //   //是否已经断开连接
      //   this.mqttError();
      //   //监听获取信息
      this.getMessage();
    },
    getMessage() {
      this.client.on("message", (topic, message) => {
        if (message) {
          console.log("收到来着", topic, "的信息", message.toString());
          // const res = JSON.parse(message.toString());
          //   switch (topic) {
          //     case "top/#":
          //       this.msg = res;
          //       break;
          //     case "three/#":
          //       this.msg = res;
          //       break;
          //     default:
          //       return;
          //   }
          //   this.msg = message;
        }
      });
    },
    roomClick(){
      
    }
  },
};
</script>
<style lang="scss">
.rooms {
  width: 100%;
  min-height: 100vh;
  background-color: rgb(239, 239, 239);
}
.room {
  padding: 20px;
  .title {
    // color: rgb(134, 134, 134);
    font-weight: bold;
    text-align: left;
  }
  .boxs {
    display: flex;
    margin-top: 20px;
    .box {
      width: 200px;
      height: 100px;
      border-radius: 15px;
      background-color: pink;
      color: #fff;
      text-align: left;
      margin-left: 20px;
      padding: 20px;
      .content {
        // margin-top: 20px;
        margin-left: 60px;
      }
    }
    .newBox {
      display: flex;
      width: 200px;
      height: 100px;
      line-height: 100px;
      border-radius: 15px;
      background-color: #fff;
      margin-left: 20px;
      padding-left: 55px;
    }
  }
}
</style>