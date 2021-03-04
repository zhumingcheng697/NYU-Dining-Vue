<template>
  <div class="Location">
    <h2>{{ location["name"] || `Unknown Location` }}</h2>
    <pre>{{ location }}</pre>
  </div>
</template>

<script>
export default {
  name: "Location",
  props: {
    location: Object
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.Location {
  margin: 10px 5px;
}
</style>
