<template>
    <div class="row">
        <div class="col-md-6 offset-md-3">
            <div id="map_canvas"></div>
        </div>
    </div>
</template>

<script>

import googleMapsInit from '../utils/gmaps';

export default {
  name: "Map",
  props: {
    position: {
      type: Object,
      default: () => ({
        lat: -30.0277,
        lng: -51.2287
      }),
    },
    address: {
      type: String,
      default: '',
    }
  },
  data: () => {
    return {
      google: null,
      geocoder: null,
      map: null,
      service: null,
    }
  },
  watch: {
    address() {
      const vm = this;
      
      let request = {
        query: vm.address,
        fields: ['name', 'geometry', 'icon']
      };

      vm.service = new vm.google.maps.places.PlacesService(vm.map);

      if (! vm.address || ! vm.address.length) {
        vm.addMarker(vm.position);
        vm.map.setCenter(vm.position);
        return false;
      }

      vm.service.findPlaceFromQuery(request, (res, status) => {
        if (status === google.maps.places.PlacesServiceStatus.OK) {
          vm.addMarker(res[0].geometry.location);
          vm.map.setCenter(res[0].geometry.location);
        }
      });

    }
  },
  async mounted() {
    try {
      const vm = this;

      // Initialize Google Maps
      vm.google = await googleMapsInit();

      // Set the map with default position and specific configurations
      vm.map = new vm.google.maps.Map(document.getElementById('map_canvas'), {
        zoom: 14,
        center: vm.position,
        disableDefaultUI: true
      });
      
    } catch (error) {
      this.$emit('error', error);
    }
  },
  methods: {
    addMarker(position = this.position) {
      const vm = this;

      let options = { 
        position: position,
        map: this.map
      };

      let marker = new vm.google.maps.Marker(options);
    }
  }
};
</script>

<style scoped>
#map_canvas {
    width: 100%; 
    height: 600px;
    padding-top: 10px;
    border: 1px;
}
</style>
