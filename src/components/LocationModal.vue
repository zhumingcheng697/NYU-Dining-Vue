<template>
  <div class="ModalLayer">
    <div class="Scrollable">
      <div class="Centered" @click="clicked">
        <div aria-modal="true" class="ModalContent">
          <LocationInfo :location-info="locationInfo"/>
          <h3 v-if="menuInfo && !Array.isArray(menuInfo)">Failed to Load Menu</h3>
          <h3 v-else-if="!menuInfo">Loading Menu…</h3>
          <h3 v-else-if="!menuInfo.length">No Menu Available</h3>
          <div v-else class="Menus">
            <h3>Menu</h3>
            <Menu v-for="(menuItem, n) in menuInfo" :key="n" :menu-item="menuItem"/>
          </div>
          <button class="ModalHideBtn" @click="$emit('hide-location-modal')" @keypress.prevent="$emit('keypress', $event)" @keypress.esc.enter.space.prevent="$emit('hide-location-modal')">×</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LocationInfo from "./LocationInfo";
import Menu from "./Menu";

export default {
  name: "LocationModal",
  props: {
    locationInfo: Object,
    menuInfo: Array || Object
  },
  components: {
    LocationInfo,
    Menu
  },
  methods: {
    clicked(e) {
      if (["ModalLayer", "Scrollable", "Centered"].some((el) => e.target.classList.contains(el))) {
        this.$emit("hide-location-modal");
      }
    }
  }
};
</script>

<style scoped>
h3 {
  padding: 0;
  margin: 18px 0 5px 0;
}

.Menus > h3 {
  margin: 18px 0 -8px 0;
}

.ModalLayer {
  background: #000a;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 10;
}

.Scrollable {
  margin: 0 auto;
  position: relative;
  overflow: auto;
  height: 100%;
  width: 100%;
}

.Centered {
  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;
  min-height: 100vh;
}

.ModalContent {
  background: #eee;
  border-radius: 12px;
  position: relative;
  margin: 30px auto;
  padding: 25px 20px 20px 20px;
  width: calc(100% - 80px - var(--inset-horizontal));
  max-width: 700px;
}

button {
  background: none;
  border: none;
  color: #888;
  cursor: pointer;
  position: absolute;
  font-family: inherit;
  font-size: 1.5em;
  margin: 0;
  top: 8px;
  right: 8px;
  width: 32px;
  height: 32px;
  padding: 0 0 4px 0;
  text-align: center;
}

.Centered:hover .ModalContent:not(:hover) button, button:hover, button:focus {
  color: #333;
}
</style>
