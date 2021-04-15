<template>
  <div class="main">
    <button @click="handleGeoloaction">获取位置</button>
    <p>{{msg}}</p>
  </div>
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    //HelloWorld
  },
  data() {
    return {
      msg: 'hello'
    }
  },
  methods: {
    handleGeoloaction() {
      if(navigator.geolocation.getCurrentPosition) {
        console.log('handleGeoloaction')
        navigator.geolocation.getCurrentPosition((pos)=>{
          var crd = pos.coords;

          console.log('Your current position is:');
          console.log('Latitude : ' + crd.latitude);
          console.log('Longitude: ' + crd.longitude);
          console.log('More or less ' + crd.accuracy + ' meters.');
        }, (err)=>{
          console.warn('ERROR(' + err.code + '): ' + err.message);
        })
      } else {
        throw new Error('您的浏览器不支持获取用户位置')
      }
    },
    getPositionSucess(position) {
      console.log('sucess', position)
      console.log(position.timestamp); // 1525364883361
      console.log(position.coords);     // Coordinates {...}
    },
    getPositionError(e) {
      console.log('error', e)
      console.log(e.code)
      console.log(e.e.message)
    }
  }
}
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
