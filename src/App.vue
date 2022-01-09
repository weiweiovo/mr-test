<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <HelloWorld />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
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

table {
  text-align: left;
  width: 100%;
}
thead tr {
  color: #fff;
  background: #42b983;
}
th,
td {
  max-width: 200px;
  padding: 10px 20px;
}
tr:nth-child(even) {
  background: #f5f5f5;
}

.text-center {
  text-align: center;
}
.flag {
  font-size: 100px;
}

.link {
  cursor: pointer;
}
.link:hover {
  color: #42b983;
}
.link:active {
  color: #349469;
}
</style>
