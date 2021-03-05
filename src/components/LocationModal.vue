<template>
  <div class="ModalLayer">
    <div class="Scrollable">
      <div class="Centered" @click="clicked">
        <div class="ModalContent">
          <LocationInfo :location-info="locationInfo"/>
          <span class="ModalHideBtn" @click="$emit('hide-location-modal')" @keypress.esc.enter.space.prevent="$emit('hide-location-modal')">×</span>
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
    locationInfo: Object
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
  border-radius: 10px;
  position: relative;
  margin: 30px auto;
  width: calc(100% - 60px - var(--inset-horizontal));
  padding: 10px 15px;
  max-width: 700px;
}

span {
  color: #666;
  cursor: pointer;
  margin: 5px 10px;
  position: absolute;
  font-size: 1.5em;
  top: 0;
  right: 5px;
}

span:hover {
  color: #333;
}

.Centered:hover .ModalContent:not(:hover) span {
  color: #333;
}
</style>
