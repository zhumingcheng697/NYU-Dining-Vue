<template>
  <div id="app">
    <h1>NYU Dining Vue</h1>
    <h2 v-if="locations && !Array.isArray(locations)">Failed to Load Locations</h2>
    <h2 v-else-if="!locations">Loading Locations…</h2>
    <h2 v-else-if="!locations.length">No Location Available</h2>
    <div v-else>
      <LocationModal v-if="locations[showingIndex]" :location-info="locations[showingIndex]" :menu-info="(menus[menuIndex(showingIndex)] || {})['menu']" @keypress="handleKeyPress" @hide-location-modal="hideModal"/>
      <div class="Locations">
        <div class="LocationBlock" :tabindex="showingIndex === -1 ? 0 : -1" v-for="(location, index) in locations" :key="location['id']" @keypress.esc="hideModal" @keypress.enter.space.prevent="toggleLocation(index)" @click="toggleLocation(index)">
          <LocationInfo :location-info="location"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LocationInfo from "./components/LocationInfo.vue";
import LocationModal from "./components/LocationModal";
import * as nodeFetch from "node-fetch";

const locationsJsonUrl = "https://s3.amazonaws.com/mobile.nyu.edu/dining/locations.json";
const menuJsonUrl = (locationId) => `https://s3.amazonaws.com/mobile.nyu.edu/dining/menus/${locationId}.json`;

export default {
  name: "App",
  data() {
    return {
      locations: null,
      menus: [],
      showingIndex: -1
    };
  },
  components: {
    LocationInfo,
    LocationModal
  },
  methods: {
    toggleLocation(index) {
      this.showingIndex = (this.showingIndex === index) ? -1 : index;

      if (this.showingIndex !== -1) {
        this.loadMenu(this.showingIndex);
        setTimeout(() => {
          document.querySelector(`.ModalHideBtn`).focus();
        }, 10);
      }
    },
    hideModal() {
      if (this.showingIndex >= 0 && this.showingIndex < this.locations.length) {
        const index = this.showingIndex + 1;
        setTimeout(() => {
          document.querySelector(`.Locations .LocationBlock:nth-child(${index})`).focus();
        }, 10);
      }

      this.showingIndex = -1;
    },
    handleKeyPress(e) {
      switch (e.key) {
        case "[":
          this.goLeft();
          break;
        case "]":
          this.goRight();
          break;
      }
    },
    goLeft() {
      if (this.showingIndex >= 0 && this.showingIndex < this.locations.length) {
        const goTo = (this.showingIndex + 14) % this.locations.length;
        this.toggleLocation(goTo);
      }
    },
    goRight() {
      if (this.showingIndex >= 0 && this.showingIndex < this.locations.length) {
        const goTo = (this.showingIndex + 1) % this.locations.length;
        this.toggleLocation(goTo);
      }
    },
    menuIndex(locationIndex) {
      return this.menus.findIndex((el) => {
        return el.id === (this.locations[locationIndex] || {})["id"];
      });
    },
    loadMenu(index) {
      setTimeout(() => {
        if (this.showingIndex === index && this.locations[index]) {
          const id = this.locations[index]["id"];

          if (id && this.menuIndex(index) === -1) {
            this.menus.push({ id: id });

            nodeFetch(menuJsonUrl(id)).then((res) => res.text()).then((text) => {
              try {
                this.menus.splice(this.menuIndex(index), 1, { id: id, menu: JSON.parse(`${text}`)["menus"] });
                this.menus[this.menuIndex(index)]["menu"].forEach((el) => {
                  delete el["filters"];
                  delete el["nutrients"];
                });
              } catch (e) {
                console.error(e);
                this.menus.splice(this.menuIndex(index), 1, { id: id, menu: {} });
              }
            });
          }
        }
      }, 500);
    }
  },
  created() {
    nodeFetch(locationsJsonUrl).then((res) => res.text()).then((text) => {
      try {
        this.locations = JSON.parse(`${text}`);
      } catch (e) {
        console.error(e);
        this.locations = {};
      }
    });
  }
};
</script>

<style>
:root {
  --inset-left: env(safe-area-inset-left, 0px);
  --inset-right: env(safe-area-inset-inset-right, 0px);
  --inset-top: env(safe-area-inset-top, 0px);
  --inset-bottom: env(safe-area-inset-bottom, 0px);
  --inset-horizontal: calc(var(--inset-left) + var(--inset-right));
}

body {
  background: #eee;
  margin: 0 auto;
  padding: calc(15px + var(--inset-top)) calc(20px + var(--inset-right)) calc(15px + var(--inset-bottom)) calc(20px + var(--inset-left));
  max-width: 1080px;
}

h2 {
  color: #57068c;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
}

.Locations {
  display: flex;
  flex-wrap: wrap;
  margin: 0 -5px;
}

.LocationBlock {
  background: #f9f9f9;
  border-radius: 12px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 5px 5px;
  padding: 10px 20px;
  width: 100%;
}

.LocationBlock:hover, .LocationBlock:focus {
  background: #fff;
}

@media (min-width: 780px) {
  .LocationBlock {
    width: calc((100% - 100px - var(--inset-horizontal)) / 2);
  }
}
</style>
