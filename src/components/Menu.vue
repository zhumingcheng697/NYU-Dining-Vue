<template>
  <div class="Menu">
    <h4>{{ menuItem["name"] || "Unknown Dish" }}</h4>
    <p class="Description" v-if="menuItem['desc'] || menuItem['ingredients']">{{ menuItem["desc"] || menuItem["ingredients"] }}</p>
    <p v-if="secondaryInfo">{{ secondaryInfo }}</p>
    <p v-if="!['desc', 'ingredients', 'calories', 'portion', 'qty'].some((el) => menuItem[el])">No Dish Information</p>
  </div>
</template>

<script>
export default {
  name: "Menu",
  props: {
    menuItem: Object
  },
  computed: {
    secondaryInfo() {
      const res = [this.menuItem["portion"] || this.menuItem["qty"]];
      if (this.menuItem["calories"]) {
        res.push(`${this.menuItem["calories"]} Calories`);
      }
      const resStr = res.filter(Boolean).join(" | ");
      return resStr && (resStr.charAt(0).toUpperCase() + resStr.slice(1));
    }
  }
};
</script>

<style scoped>
h4 {
  padding: 0;
  margin: 18px 0 5px 0;
}

p {
  color: #555;
  margin: 6px 0;
  padding: 0;
}

.Description {
  color: #333;
  font-style: italic;
}
</style>
