<template>
  <div class="LocationInfo">
    <h2>{{ locationInfo["name"] || "Unknown Location" }}</h2>
    <strong>{{ address || "No Address Information" }}</strong>
    <p v-if="!schedules.length">No Schedule Information</p>
    <Schedule v-else v-for="([days, sessions], key) in schedules" :key="key" :days="days" :sessions="sessions"/>
  </div>
</template>

<script>
import Schedule from "./Schedule.vue";

export default {
  name: "LocationInfo",
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
            return el["type"] === "location" && el["days"] && Array.isArray(el["days"]) && el["days"].length && el["start_hour"] && el["end_hour"];
          }).reduce((prev, curr) => {
            const dayKey = curr["days"].slice().sort((a, b) => {
              return ((a === 0 ? 7 : a) - (b === 0 ? 7 : b));
            }).join(",");

            if (!prev[dayKey]) {
              prev[dayKey] = [];
            }

            prev[dayKey].push({ start: curr["start_hour"], end: curr["end_hour"] });
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
      }).map(([days, sessions]) => {
        return [days, sessions.sort(({ start: start1 }, { start: start2 }) => {
          return start1 - start2;
        })];
      });
    }
  }
};
</script>

<style scoped>
h2 {
  color: #57068c;
  margin: 12px 0;
  padding: 0;
}

strong {
  display: block;
  margin: 12px 0;
  padding: 0;
}

p {
  color: #333;
  margin: 6px 0;
  padding: 0;
}
</style>
