<template>
  <OfficeDoor msg="XENON SMART"/>
</template>

<script>
import OfficeDoor from './components/OfficeDoor.vue'

export default {
  name: 'App',
  components: {
    OfficeDoor
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
  /* margin-top: 150px; */
}

html, body {
  width: 100%;
  height: 100%;
  overflow: hidden;
  margin: 0px;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
