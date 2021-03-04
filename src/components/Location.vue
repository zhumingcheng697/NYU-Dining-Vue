<template>
  <div class="Location">
    <h2>{{ locationInfo["name"] || "Unknown Location" }}</h2>
    <strong>{{ address || "No Address Information" }}</strong>
    <p v-if="!schedules.length">No Schedule Information</p>
    <Schedule v-else v-for="([days, sessions], key) in schedules" :key="key" :days="days" :sessions="sessions"/>
<!--    <pre v-for="([days, schedule], key) in schedules" :key="key">{{ days }} {{ schedule }}</pre>-->
  </div>
</template>

<script>
import Schedule from "./Schedule.vue";

export default {
  name: "Location",
  props: {
    locationInfo: Object
  },
  components: {
    Schedule
  },
  computed: {
    address() {
      const { street, city, state, zip_code } = (this.locationInfo["address"] || {});
      return [street, city, state].filter(Boolean).join(", ") + (zip_code ? ` ${zip_code}` : "");
    },
    schedules() {
      return Object.entries(
          (this.locationInfo["schedules"] || []).filter((el) => {
            return el["type"] === "location" && el["days"] && Array.isArray(el["days"]) && el["start_hour"] && el["end_hour"];
          }).reduce((prev, curr) => {
            const dayKey = curr["days"].sort((a, b) => {
              return ((a === 0 ? 7 : a) - (b === 0 ? 7 : b));
            }).join(",");

            if (!prev[dayKey]) {
              prev[dayKey] = [];
            }

            prev[dayKey].push({ start: curr["start_hour"], end: curr["end_hour"] });
            prev[dayKey].sort(({ start: start1 }, { start: start2 }) => {
              return start1 - start2;
            });
            return prev;
          }, {})
      ).sort((a, b) => {
        if ((a[0] === "0" ? "7" : a[0]) < (b[0] === "0" ? "7" : b[0])) {
          return -1;
        } else if ((a[0] === "0" ? "7" : a[0]) > (b[0] === "0" ? "7" : b[0])) {
          return 1;
        } else {
          return 0;
        }
      });
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
