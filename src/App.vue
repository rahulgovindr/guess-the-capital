<template>
  <div id="app">
    <!-- <div class="background"></div> -->
    <Home />
  </div>
</template>

<script>
import Home from './components/Home.vue';

export default {
  name: 'App',
  components: {
    Home
}
}
</script>

<style>
body {
  margin: 0px;
}
.background {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-image: url('https://picsum.photos/1920/1080');
        background-size: cover;
        filter: blur(5px);
      }
</style>
