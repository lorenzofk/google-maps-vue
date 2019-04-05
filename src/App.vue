<template>
  <div id="app">
      <SearchBar 
        @search="searchAddress($event)" 
        @error="showError('searchBar', $event)"
        @clear="clearAddress()">
      </SearchBar>
      <hr>
      <Map
        @error="showError('map', $event)"
        :address="query">
      </Map>
  </div>
</template>

<script>
  import Map from './components/Map';
  import SearchBar from './components/SearchBar';

  export default {
    name: "App",
    components: { SearchBar, Map },
    data: () => {
      return {
        query: ''
      }
    },
    methods: {
      clearAddress() {
        this.query = '';
      },
      searchAddress(address) {
        this.query = address;
      },
      showError(type, msg) {
        switch (type) {
          case 'map':
            console.log(`Error ocurred while rendering Google Maps: ${msg}`);
            break;
          case 'searchBar':
            console.log(`Error ocurred while searching an address: ${msg}`);
            break;
        }
      },
    },
  };

</script>

<style>
body {
  background-color: #007bff2b !important;
}
h4 {
  margin-top: 10px;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
