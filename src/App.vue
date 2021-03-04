<template>
  <div id="app">
    <h1>NYU Dining Vue</h1>
    <Location v-for="location in locations" :key="location['id']" :locationInfo="location"/>
  </div>
</template>

<script>
import Location from "./components/Location.vue";
// import * as nodeFetch from "node-fetch";

// const locationsJsonUrl = "https://s3.amazonaws.com/mobile.nyu.edu/dining/locations.json";

export default {
  name: "App",
  data() {
    return {
      locations: []
    };
  },
  components: {
    Location
  },
  created() {
    // nodeFetch(locationsJsonUrl).then((res) => res.text()).then((text) => {
    //   try {
    //     const locations = JSON.parse(`${text}`);
    //     locations.forEach((locationInfo) => {
    //       delete locationInfo["schedules"];
    //     });
    //     this.locations = locations;
    //     console.log(JSON.stringify(locations));
    //   } catch(e) {
    //     console.error(e);
    //     this.locations = null;
    //   } finally {
    //     console.log(this.locations);
    //   }
    // });
    this.locations = JSON.parse(`[{"address":null,"id":"5fd7a3ff31d1ee16e015be9b","name":"Bridgeview Market","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 10:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.9981099,40.7296364],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.729734769927944,"lon":-73.99547403005943,"manual_coords":[40.729734769927944,-73.99547403005943],"metadata":"60 Washington Square South","phone":null,"phone_formatted":null,"state":"New York","street":"60 Washington Square South","zip_code":"10012","zone_name":null},"id":"5d2fa5e44198d4071b84eee0","name":"The Marketplace at Kimmel","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 7:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.9891438,40.7339346],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7339346,"lon":-73.9891438,"manual_coords":[],"metadata":"110 E 14th Street","phone":null,"phone_formatted":null,"state":"New York","street":"110 E 14th Street","zip_code":"10003","zone_name":null},"id":"5d699c7cc4b7ff0fa249d9ca","name":"Dunkin'","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 6:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"Brooklyn","coordinates":[-73.98654028773304,40.69422419919221],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.69422419919221,"lon":-73.98654028773304,"manual_coords":[40.69422419919221,-73.98654028773304],"metadata":"6 MetroTech Center","phone":"7182603786","phone_formatted":"(718) 260 - 3786","state":"New York","street":"6 MetroTech Center","zip_code":"11201","zone_name":null},"id":"5d2fc6bb4198d4071b89cb42","name":"Jasper Kane Cafe","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 10:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.9952772660362,40.73088610044049],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.73088610044049,"lon":-73.9952772660362,"manual_coords":[null,null],"metadata":"5-11 University Pl","phone":null,"phone_formatted":null,"state":"NY","street":"5-11 University Pl","zip_code":"10003","zone_name":null},"id":"5d477f3c1ca48e0934e57625","name":"Kosher Eatery","open":false,"status":{"color":"red","label":"closed","message":"Closed. Opens at 11:30am."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.9993256,40.7315278],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7315278,"lon":-73.9993256,"manual_coords":[null,null],"metadata":"33 Washington Square W","phone":null,"phone_formatted":null,"state":"NY","street":"33 Washington Square W","zip_code":"10011","zone_name":null},"id":"5d4782ab4198d40988e84a9d","name":"Market at Lipton","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 3:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[null,null],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":null,"lon":null,"manual_coords":[null,null],"metadata":null,"phone":null,"phone_formatted":null,"state":"New York","street":"5 University Pl #11","zip_code":"10003","zone_name":null},"id":"5d4782ab4198d40988e84a9c","name":"Market at Sidestein","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 11:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.995004580778,40.7310066],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7310066,"lon":-73.995004580778,"manual_coords":[null,null],"metadata":"Bldg #11","phone":null,"phone_formatted":null,"state":"New York","street":"5 University Pl","zip_code":"10003","zone_name":null},"id":"5d2faa744198d4072d6457a8","name":"NYU EATS at Downstein","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 3:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York City","coordinates":[-73.9993256,40.7315278],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7315278,"lon":-73.9993256,"manual_coords":[null,null],"metadata":"Hayden Residence Hall","phone":null,"phone_formatted":null,"state":"New York","street":"33 Washington Square W","zip_code":"10011","zone_name":null},"id":"5d477f3b1ca48e0934e57624","name":"NYU EATS at Lipton","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 3:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.9883333,40.7330556],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7330556,"lon":-73.9883333,"manual_coords":[],"metadata":null,"phone":null,"phone_formatted":null,"state":"New York","street":"140 E 14th St","zip_code":"10003","zone_name":null},"id":"5d8270501ca48e0d1f718eb3","name":"Palladium","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 3:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.9981099,40.7296364],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.729750014176965,"lon":-73.99546464232787,"manual_coords":[40.729750014176965,-73.99546464232787],"metadata":"60 Washington Square S","phone":null,"phone_formatted":null,"state":"New York","street":"60 Washington Square S","zip_code":"10012","zone_name":null},"id":"5d41cd16c4b7ff0cad8363e0","name":"Peet's Coffee","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 4:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"NY","coordinates":[-73.9954637,40.7290097],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.729576111716796,"lon":-73.99652350244673,"manual_coords":[40.729576111716796,-73.99652350244673],"metadata":"45 W 4th St","phone":null,"phone_formatted":null,"state":"New York","street":"45 W 4th St","zip_code":"10012","zone_name":null},"id":"5d4321e31ca48e0a15403ac6","name":"Starbucks","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 9:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.995004580778,40.7310066],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7310066,"lon":-73.995004580778,"manual_coords":[],"metadata":"1st floor, Building #11","phone":null,"phone_formatted":null,"state":"New York","street":"5 University Place","zip_code":"1003","zone_name":null},"id":"5d6997e7c4b7ff0f99897ee1","name":"Upstein Chick-fil-A","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 8:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.995004580778,40.7310066],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7310066,"lon":-73.995004580778,"manual_coords":[],"metadata":"1st floor, Building # 11","phone":null,"phone_formatted":null,"state":"New York","street":"5 University Place","zip_code":"10003","zone_name":null},"id":"5d699a59c4b7ff0fab12a297","name":"Upstein Daily Press","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 8:00pm."},"type":"webtrition"},{"address":{"abbreviation":null,"city":"New York","coordinates":[-73.995004580778,40.7310066],"dst":false,"ext_formatted":null,"gmt":0,"gmt_offset":0,"lat":40.7310066,"lon":-73.995004580778,"manual_coords":[],"metadata":"1st floor, Building #11","phone":null,"phone_formatted":null,"state":"New York","street":"5 University Place","zip_code":"10003","zone_name":null},"id":"5d699aa74198d411c6ff853c","name":"Upstein The Egg Shoppe","open":true,"status":{"color":"green","label":"open","message":"Open. Closes at 8:00pm."},"type":"webtrition"}]
`);
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 10px;
}
</style>
