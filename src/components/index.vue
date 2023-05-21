<template>
  <div></div>
</template>
<script>
import mqtt from "mqtt";
export default {
  name: "indexVue",
  data() {
    return {
      client: {},
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
        port: 8083,
      };
        // this.client = mqtt.connect("ws://broker.emqx.io/mqtt", options);
      this.client = mqtt.connect("ws://nuc.scut.mcurobot.com/mqtt", options);
      this.client.on("connect", (e) => {
        console.log("成功连接服务器:", e);
        //订阅三个名叫'top/#', 'three/#'和'#'的主题
        this.client.subscribe(['room1_sr', 'room11_sr','room111_sr'], { qos: 1 }, (err) => {
          if (!err) {
            console.log("订阅成功");
          } else {
            console.log("消息订阅失败！");
          }
        });
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
  },
};
</script>
<style lang="">
</style>