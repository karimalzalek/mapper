<template>
  <div id="app">
    <h1>Map Location Search and Routing</h1>
    <div class="controls">
      <input v-model="searchQuery" placeholder="Search location by name" @keyup.enter="searchLocation" />
      <input v-model="latitude" type="number" placeholder="Latitude" />
      <input v-model="longitude" type="number" placeholder="Longitude" />
      <button @click="addMarkerByCoords">Add Marker by Coordinates</button>
      <button @click="createShortestRoute">Create Shortest Route</button>
    </div>
    <table>
      <thead>
        <tr>
          <th>#</th>
          <th>Location</th>
          <th>Latitude</th>
          <th>Longitude</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(marker, index) in markers" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ marker.name || 'Coordinates' }}</td>
          <td>{{ marker.lat }}</td>
          <td>{{ marker.lng }}</td>
        </tr>
      </tbody>
    </table>
    <div ref="mapContainer" class="map-container"></div>
  </div>
</template>

<script>
import mapboxgl from 'mapbox-gl';
import mbxGeocoding from '@mapbox/mapbox-sdk/services/geocoding';
import mbxDirections from '@mapbox/mapbox-sdk/services/directions';

export default {
  data() {
    return {
      map: null,
      searchQuery: '',
      latitude: null,
      longitude: null,
      markers: [],
      directionsClient: null,
    };
  },
  mounted() {
    mapboxgl.accessToken = 'pk.eyJ1Ijoia2V5bW8iLCJhIjoiY2p2ZGhwZGRoMDVmMzQ2cXVxeHIxd2MxayJ9.wCZdKhGLUafZCcCvN-57Nw';
    this.map = new mapboxgl.Map({
      container: this.$refs.mapContainer,
      style: 'mapbox://styles/mapbox/streets-v11',
      center: [-122.4194, 37.7749], // Default center (San Francisco)
      zoom: 10,
    });
    this.directionsClient = mbxDirections({ accessToken: mapboxgl.accessToken });
  },
  methods: {
    async searchLocation() {
      const geocodingClient = mbxGeocoding({ accessToken: mapboxgl.accessToken });
      const response = await geocodingClient
        .forwardGeocode({
          query: this.searchQuery,
          limit: 1,
        })
        .send();
      const match = response.body.features[0];
      if (match) {
        this.addMarker(match.center[1], match.center[0], match.place_name);
      }
    },
    addMarkerByCoords() {
      if (this.latitude && this.longitude) {
        this.addMarker(this.latitude, this.longitude);
      }
    },
    addMarker(lat, lng, name = '') {
      const marker = new mapboxgl.Marker().setLngLat([lng, lat]).addTo(this.map);
      this.markers.push({ lat, lng, name, marker });
      this.updateMarkerTable();
    },
    updateMarkerTable() {
      this.markers = this.markers.map((marker, index) => ({
        ...marker,
        sequence: index + 1,
      }));
    },
    async createShortestRoute() {
      if (this.markers.length < 2) return;

      const coordinates = this.markers.map(marker => [marker.lng, marker.lat]);
      const response = await this.directionsClient
        .getDirections({
          profile: 'driving-traffic',
          waypoints: coordinates.map(coord => ({ coordinates: coord })),
          geometries: 'geojson',
        })
        .send();
      const route = response.body.routes[0].geometry;
      
      this.drawRoute(route);
      this.updateRouteSequence(response.body.waypoints);
    },
    drawRoute(route) {
      if (this.map.getSource('route')) {
        this.map.removeLayer('route');
        this.map.removeSource('route');
      }
      this.map.addSource('route', {
        type: 'geojson',
        data: {
          type: 'Feature',
          geometry: route,
        },
      });
      this.map.addLayer({
        id: 'route',
        type: 'line',
        source: 'route',
        layout: {
          'line-join': 'round',
          'line-cap': 'round',
        },
        paint: {
          'line-color': '#1db7dd',
          'line-width': 5,
        },
      });
    },
    updateRouteSequence(waypoints) {
      this.markers = waypoints.map((point, index) => ({
        ...this.markers.find(marker => marker.lat === point.location[1] && marker.lng === point.location[0]),
        sequence: index + 1,
      }));
      this.updateMarkerTable();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  max-width: 900px;
  margin: 0 auto;
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 15px;
}

table, th, td {
  border: 1px solid #ccc;
}

th, td {
  padding: 8px;
  text-align: center;
}

.map-container {
  width: 100%;
  height: 400px;
}
</style>
