<template>
  <div class="container-fluid h-100  al-s8-converter-bg" id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "app",
  components: {
    HelloWorld
  }
};
</script>

<style>
.bg-fullscreen {
  height: 100%;
}

.al-s8-converter-bg {
  background: url(assets/Illo1.jpg) no-repeat;
  background-position: center 30%;
  background-attachment: fixed;
  background-size: cover;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center;
  color: #2c3e50;
  margin-top: 60px; */
}
</style>
