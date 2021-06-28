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
    // 调用通知方法
			showMsgNotification('状态更新提醒','你的朋友圈有3条新状态，快去查看吧');
			/**
			 * H5通知功能 
			 */
			function showMsgNotification(out_title, out_msg) {
				var title = out_title ? out_title : '更新状态标题';
				var options = {
					body: out_msg ? out_msg : "更新状态内容", // 通知主体
					requireInteraction: true, // 不自动关闭通知
					icon: 'http://img18.house365.com/newcms/2017/03/16/148964317858ca26aacf7b5.jpg', // 通知图标 
					tag: 'hangge',
				};
				var Notification = window.Notification || window.mozNotification || window.webkitNotification; // 浏览器做兼容处理
				if(Notification) { //支持桌面通知
					if(Notification.permission == "granted") { //已经允许通知
						instance_init(title, options);
					} else {
						//第一次询问或已经禁止通知(如果用户之前已经禁止显示通知，那么浏览器不会再次询问用户的意见，Notification.requestPermission()方法无效)
						Notification.requestPermission(function(status) {
							if(status === "granted") { //用户允许
								instance_init(title, options);
							} else { //用户禁止
								console.log('禁止')
								return false
							}
						});
					}
					/**
					 * Notification定义
					 * */
					function instance_init(title, options){
						var instance = new Notification(title, options);
						instance.onclick = function() {
							console.log('onclick');
							// 关闭通知
							instance.close();
						};
						instance.onerror = function() {
							console.log('onerror');
						};
						instance.onshow = function() {
							console.log('onshow');
						};
						instance.onclose = function() {
							console.log('onclose');
						};
					}
				} else { //不支持(IE等)
					console.log("不支持的浏览器")
				}
			}
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
