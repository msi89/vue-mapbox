<template>
  <div id="app">
    <div class="map">
      <Map :markers="coordonates" />
    </div>
  </div>
</template>

<script>
import Map from './components/Map'

export default {
  components: {
    Map
  },
  data() {
    return {
      coordonates: [
        { lat: 55.75222, lng: 37.61556, color: 'coral' },
        { lng: -111.549668, lat: 39.014, color: 'green' }
      ]
    }
  },
  created() {},
  methods: {}
}
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
</style>
