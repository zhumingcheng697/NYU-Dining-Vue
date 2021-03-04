<template>
  <div class="Location">
    <h2>{{ locationInfo["name"] || "Unknown Location" }}</h2>
    <p>{{ address || "No Address Information" }}</p>
    <pre>{{ locationInfo }}</pre>
  </div>
</template>

<script>
export default {
  name: "Location",
  props: {
    locationInfo: Object
  },
  computed: {
    address() {
      const { street, city, state, zip_code } = (this.locationInfo["address"] || {});
      return [street, city, state].filter(Boolean).join(", ") + (zip_code ? ` ${zip_code}` : "");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.Location {
  margin: 10px 5px;
}
</style>
