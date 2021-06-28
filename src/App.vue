<template>
  <div class="main">
    <button @click="handleGeoloaction">获取位置</button>
    <h1>经度: {{Longitude}}</h1>
    <h1>纬度: {{Latitude}}</h1>
  </div>

  <p>{{msg}}</p>
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'

export default {
  name: "App",
  components: {
    //HelloWorld
  },
  data() {
    return {
      Longitude: "",
      Latitude: "",
      msg: ""
    };
  },
  mounted() {
    Notification.requestPermission(function(status) {
      if (status === "granted") {
        //用户允许
        var instance = new Notification("title", {
          body: "Body text!",
          image: 'http://img18.house365.com/newcms/2017/03/16/148964317858ca26aacf7b5.jpg', // 通知图标 
          requireInteraction: true, // 不自动关闭通知
          icon: 'http://img18.house365.com/newcms/2017/03/16/148964317858ca26aacf7b5.jpg', // 通知图标 
          vibrate: true
        });
        instance.onclick = function() {
          console.log("onclick");
          // 关闭通知
          instance.close();
        };
        instance.onerror = function() {
          console.log("onerror");
        };
        instance.onshow = function() {
          console.log("onshow");
        };
        instance.onclose = function() {
          console.log("onclose");
        };
      } else {
        //用户禁止
        console.log("禁止");
        return false;
      }
    });
    // Notification.requestPermission().then(permission => {
    //   console.log("User responded to permission request:", permission);
    //   let n = new Notification("Title text!", {
    //     body: "Body text!",
    //     image: "path/to/image.png",
    //     vibrate: true
    //   });
    //   setTimeout(() => n.close(), 5000);
    // });
  },
  methods: {
    handleGeoloaction() {
      if (navigator.geolocation.getCurrentPosition) {
        console.log("handleGeoloaction");
        this.msg = "init";
        navigator.geolocation.getCurrentPosition(
          pos => {
            this.msg = "done";
            var crd = pos.coords;

            console.log("Your current position is:");
            console.log("Latitude : " + crd.latitude);
            console.log("Longitude: " + crd.longitude);
            console.log("More or less " + crd.accuracy + " meters.");
            this.Longitude = crd.longitude;
            this.Latitude = crd.latitude;
          },
          err => {
            this.msg = "err";
            console.warn("ERROR(" + err.code + "): " + err.message);
          }
        );
      } else {
        throw new Error("您的浏览器不支持获取用户位置");
      }
    },
    getPositionSucess(position) {
      console.log("sucess", position);
      console.log(position.timestamp); // 1525364883361
      console.log(position.coords); // Coordinates {...}
    },
    getPositionError(e) {
      console.log("error", e);
      console.log(e.code);
      console.log(e.e.message);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
