<template>
  <VideoChat />
</template>

<script>
import VideoChat from './components/VideoChat.vue'

export default {
  name: 'App',
  components: {
    VideoChat
  }
}
</script>

<style>
body, html {
    background-color: gray;
    height: 100%;
}
 
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 90%;
  margin-top: auto;
  margin-bottom: auto;
  padding: 10px;
}
</style>
