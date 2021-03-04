<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import * as nodeFetch from "node-fetch";

const locationsJsonUrl = "https://s3.amazonaws.com/mobile.nyu.edu/dining/locations.json";

export default {
  name: "App",
  data() {
    return {
      locations: []
    };
  },
  components: {
    HelloWorld
  },
  created() {
    nodeFetch(locationsJsonUrl).then((res) => res.text()).then((text) => {
      try {
        const locations = JSON.parse(`${text}`);
        locations.forEach((location) => {
          delete location["schedules"];
        });
        this.locations = locations;
      } catch(e) {
        console.error(e);
        this.locations = null;
      } finally {
        console.log(this.locations);
      }
    });
  }
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
</style>
