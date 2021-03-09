<template>
  <div class="ModalLayer">
    <div class="Scrollable">
      <div class="Centered" @click="clicked">
        <div aria-modal="true" class="ModalContent">
          <LocationInfo :location-info="locationInfo"/>
          <h3 v-if="menuInfo && !Array.isArray(menuInfo)">Failed to Load Menu</h3>
          <h3 v-else-if="!menuInfo">Loading Menu…</h3>
          <h3 v-else-if="!menuInfo.length">No Menu Available</h3>
          <pre v-else>{{ menuInfo }}</pre>

          <button class="ModalHideBtn" @click="$emit('hide-location-modal')" @keypress.prevent="$emit('keypress', $event)" @keypress.esc.enter.space.prevent="$emit('hide-location-modal')">×</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LocationInfo from "./LocationInfo";

export default {
  name: "LocationModal",
  props: {
    locationInfo: Object,
    menuInfo: Array || Object,
  },
  components: {
    LocationInfo
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
  padding: 0 0 3px 0;
}

.Centered:hover .ModalContent:not(:hover) button, button:hover, button:focus {
  color: #333;
}
</style>
