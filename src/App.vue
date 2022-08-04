<template>
  <Map/>
</template>

<script>
import Map from './components/Map.vue';

export default {
  name: 'App',
  components: {
    Map,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  user-select: none;
}
body {
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}
</style>
