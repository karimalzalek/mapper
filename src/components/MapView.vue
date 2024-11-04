<template>
  <div class="map-container" ref="mapContainer"></div>
</template>

<script>
import mapboxgl from 'mapbox-gl';

export default {
  name: 'MapView',
  data() {
    return {
      map: null,
    };
  },
  mounted() {
    mapboxgl.accessToken = 'pk.eyJ1Ijoia2V5bW8iLCJhIjoiY2p2ZGhwZGRoMDVmMzQ2cXVxeHIxd2MxayJ9.wCZdKhGLUafZCcCvN-57Nw'; // Replace with your Mapbox token
    this.map = new mapboxgl.Map({
      container: this.$refs.mapContainer,
      style: 'mapbox://styles/mapbox/streets-v11',
      center: [0, 0],
      zoom: 2,
    });
  },
};
</script>

<style scoped>
.map-container {
  width: 100%;
  height: 500px;
}
</style>
