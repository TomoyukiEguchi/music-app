<template>
  <div class="iphoneX">
    <MusicApp />
  </div>
</template>

<script>
import MusicApp from './MusicAppScreen.vue'

export default {
  components: {
    MusicApp
  }
}
</script>

<style scoped>
body, html {
  margin: 0;
  padding: 0;
}
.iphoneX {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  width: 280px;
  height: 540px;
  background: #fff;
  border-radius: 35px;
  box-shadow: inset 0 0 15px rgba(0,0,0,.2), 0 40px 100px rgba(0,0,0,.7);
  transition: .5s;
}
.iphoneX:before {
  content: '';
  position: absolute;
  top: 15px;
  left: 15px;
  width: calc(100% - 30px);
  height: calc(100% - 30px);
  /*background: #7e7e7e;*/
  border-radius: 25px;
  box-shadow: inset 0 0 20px rgba(0,0,0,.2);
  transition: .5s;
}
.iphoneX:after {
  content: '';
  position: absolute;
  top: 14px;
  left: 50%;
  transform: translateX(-50%);
  width: 130px;
  height: 16px;
  background: #fff;
}
</style>