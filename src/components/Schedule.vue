<template>
  <p>{{ daysStr }}: {{ sessionsStr }}</p>
</template>

<script>
const daysOfWeek = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

export default {
  name: "Schedule",
  props: {
    days: String,
    sessions: Array
  },
  computed: {
    daysStr() {
      const daysArr = this.days.split(",").map((n) => daysOfWeek[n % 7]);
      return daysArr.length === 1 ? daysArr[0] : `${daysArr[0]} - ${daysArr[daysArr.length - 1]}`;
    },
    sessionsStr() {
      return this.sessions.map(({ start, end }) => {
        const hourStr = (hour) => {
          if (hour >= 0 && hour <= 11 || hour === 24) {
            return `${hour % 12 === 0 ? 12 : (hour % 12)}AM`;
          } else if (hour >= 12 && hour <= 23) {
            return `${hour % 12 === 0 ? 12 : (hour % 12)}PM`;
          } else {
            return `${hour}`;
          }
        };
        return `${hourStr(start)} - ${hourStr(end)}`;
      }).join(", ");
    }
  }
};
</script>

<style scoped>

</style>
