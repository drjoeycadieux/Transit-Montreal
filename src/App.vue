<template>
  <div class="header">
    <p>Transit Montreal</p>
  </div>
  <div>
    <div v-if="error" class="error">{{ error }}</div>
    <div v-else>
      <div ref="map" class="map"></div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import mapboxgl from "mapbox-gl";

export default {
  data() {
    return {
      error: null,
      gtfsData: null,
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      const apiKey = "l7130aafef7c6e481e9aa9fc1c412dd5c9"; // Replace with your actual API key
      const apiUrl = `https://api.stm.info/pub/od/gtfs-rt/ic/v2?key=${apiKey}`;

      axios
        .get(apiUrl, { withCredentials: false })
        .then((response) => {
          this.gtfsData = response.data;

          // Initialize Mapbox
          mapboxgl.accessToken =
            "pk.eyJ1Ijoiam9leWNyZWF0b3IiLCJhIjoiY2xwbGR4bmNqMDNmcDJqcGV1OXY4Z2hteiJ9.yMegQFr9KWFFEsgmdbFRyg";
          const map = new mapboxgl.Map({
            container: this.$refs.map,
            style: "mapbox://styles/mapbox/dark-v10",
            center: [-73.5673, 45.5017], // Montreal coordinates (adjust as needed)
            zoom: 12,
          });

          // Use GTFS-RT data to add markers, layers, etc., on the map
          this.addMarkers(map);
        })
        .catch((error) => {
          this.error = `Error fetching data: ${error.message}`;
        });
    },
    addMarkers(map) {
      // Example: Add a marker for each vehicle in the GTFS-RT data
      if (this.gtfsData) {
        this.gtfsData.entities.forEach((vehicle) => {
          const coordinates = [vehicle.longitude, vehicle.latitude];
          new mapboxgl.Marker().setLngLat(coordinates).addTo(map);
        });
      }
    },
  },
};
</script>

<style>
#map {
  width: 100%;
  height: 100%;
  position: absolute;
}
.error {
  color: red;
}
</style>
