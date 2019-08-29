<template>
  <div id="app">
    <tb/>
  </div>
</template>

<script>
import tb from './components/tb'
export default {
  name: 'app',
  components: {
    tb
  }
}
</script>

<style lang="scss" style="text/css">
@import url('./assets/css/init.css');
@import url('./assets/css/table.css');
html,
body,
#app {
  height: 100%;
}
</style>
