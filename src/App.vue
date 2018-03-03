<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <span class="icon-add_circle"></span>
  </div>
</template>

<script>
  const data = {
    msg: "Hello Vue!"
  }
  export default {
    name: 'App',
    data () {
      return data;
    }
  }
</script>

<style>
  @import "./common/stylus/icon.css";
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}
</style>
