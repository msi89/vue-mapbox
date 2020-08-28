<template>
  <div id="app">
    <div class="map">
      <MglMap
        :accessToken="accessToken"
        :mapStyle.sync="mapStyle"
        @load="onMapLoaded"
        :center="center"
        :zoom="zoom"
        :speed="speed"
      >
        <div class="map-native-controls">
          <MglGeolocateControl
            position="bottom-left"
            :trackUserLocation="false"
          />
        </div>

        <div class="map-controls">
          <div class="map-zoom-controls">
            <button @click="zoomIn()">&#43;</button>
            <div class="divider" />
            <button @click="zoomOut()">&#8722;</button>
          </div>
          <button class="btn-locate" @click="locate()">
            <svg viewBox="0 0 512 512">
              <path
                d="m8.828125 282.484375h45.902344c12.0625 91.066406 83.71875 162.722656 174.785156 174.785156v45.902344c.015625 4.871094 3.960937 8.8125 8.828125 8.828125h35.3125c4.867188-.015625 8.8125-3.957031 8.828125-8.828125v-45.902344c91.066406-12.0625 162.722656-83.71875 174.785156-174.785156h45.902344c4.871094-.015625 8.8125-3.960937 8.828125-8.828125v-35.3125c-.015625-4.867188-3.957031-8.8125-8.828125-8.828125h-45.902344c-12.0625-91.066406-83.71875-162.722656-174.785156-174.785156v-45.902344c-.015625-4.871094-3.960937-8.8125-8.828125-8.828125h-35.3125c-4.867188.015625-8.8125 3.957031-8.828125 8.828125v45.902344c-91.066406 12.0625-162.722656 83.71875-174.785156 174.785156h-45.902344c-4.871094.015625-8.8125 3.960937-8.828125 8.828125v35.3125c.015625 4.867188 3.957031 8.8125 8.828125 8.828125zm247.171875-176.554687c82.878906 0 150.070312 67.191406 150.070312 150.070312s-67.191406 150.070312-150.070312 150.070312-150.070312-67.191406-150.070312-150.070312c.117187-82.832031 67.238281-149.953125 150.070312-150.070312zm0 0"
              />
              <path
                d="m326.621094 256c0 39.003906-31.617188 70.621094-70.621094 70.621094s-70.621094-31.617188-70.621094-70.621094 31.617188-70.621094 70.621094-70.621094 70.621094 31.617188 70.621094 70.621094zm0 0"
              />
            </svg>
          </button>
        </div>
        <Point
          v-for="(marker, m) in markers"
          :key="m"
          :latitude="marker.lat"
          :longitude="marker.lng"
          :color="marker.color || 'blue'"
          @onMarkerClick="onMarkerClick"
        >
          <div class="card">
            <div class="title">Hello, I'm popup!</div>
            <div class="desc">
              Lorem ipsum dolor, sit amet consectetur adipisicing elit. Dolores
              recusandae, voluptatibus dolore consequatur cumque veritatis
              consectetur autem distinctio fugiat obcaecati eveniet nesciunt
              perspiciatis temporibus aperiam laborum odit delectus, quod velit!
            </div>
          </div>
        </Point>
      </MglMap>
    </div>
  </div>
</template>

<script>
// source: https://soal.github.io/vue-mapbox
import Mapbox from 'mapbox-gl'
import Point from './Marker'
import { MglMap, MglGeolocateControl } from 'vue-mapbox'

export default {
  components: {
    MglMap,
    MglGeolocateControl,
    Point
  },
  props: {
    markers: { type: Array, default: () => [] },
    center: {
      type: Array,
      default: () => [37.618423, 55.751244] /* [longitude, latitude] */
    },
    zoom: { type: [Number, String], default: 12 },
    speed: { type: [Number, String], default: 1 }
  },
  data() {
    return {
      map: null,
      accessToken:
        'pk.eyJ1IjoibXNpODkiLCJhIjoiY2pxZ3pkZzdoNTQ1NjQzcHB1cnozY3ZuYiJ9.r8-AsWcESYVuNUe-josDZw', // your access token. Needed if you using Mapbox maps
      mapStyle: 'mapbox://styles/mapbox/streets-v11'
    }
  },
  created() {
    // We need to set mapbox-gl library here in order to use it in template
    this.mapbox = Mapbox
  },
  computed: {},
  methods: {
    onMapLoaded(event) {
      this.map = event.component
      // this.flyTo(coordinates.join(','))
    },
    async flyTo(lat, lng) {
      await this.map.actions.flyTo({
        center: [lng, lat],
        zoom: this.zoom,
        speed: this.speed
      })
    },
    zoomIn() {
      this.map.actions.zoomIn()
    },
    zoomOut() {
      this.map.actions.zoomOut()
    },
    locate() {
      console.log(this.map)
      console.log(this.mapbox)

      const gps = document.querySelector('.mapboxgl-ctrl-geolocate')
      console.log(gps)
      gps.click()
    },
    onMarkerClick(lat, lng) {
      this.flyTo(lat, lng)
      console.log(lat, '-', lng)
    }
  }
}
</script>

<style lang="scss" scoped>
$control-width: 30px;
$control-box-shadow: 0 0 2px #999;
$control-border: 1px solid #eee;
$control-color: rgba(0, 0, 0, 0.5);
$control-color-hover: rgba(0, 0, 0, 0.6);

.map {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;

  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }
  .map-native-controls {
    position: relative;
    visibility: hidden;
  }
  .map-controls {
    position: absolute;
    top: 10px;
    right: 10px;
    display: block;
    button {
      outline: none;
      border: none;
      background: transparent;
      width: $control-width;
      height: $control-width;
      cursor: pointer;
      font-weight: 700;
      font-size: 1.1rem;
      color: $control-color;
      transition: all 0.2s;

      svg {
        fill: $control-color;
        width: 16px;
        height: 16px;
        transition: all 0.2s;
      }

      &:hover {
        color: $control-color-hover;
        svg: {
          fill: $control-color-hover;
        }
      }
    }
    .btn-locate {
      background: #fff;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 20px;
      border: $control-border;
      margin: 10px 0;
      box-shadow: $control-box-shadow;
    }
    .map-zoom-controls {
      width: 30px;
      background: #fff;
      border: $control-border;
      border-radius: 20px;
      box-shadow: $control-box-shadow;

      .divider {
        width: 25px;
        height: 0.025rem;
        background: #eee;
        margin: 0px auto;
      }
    }
  }
}
.card {
  width: 200px;
  .title {
    padding: 20px;
    background: rebeccapurple;
    color: #fff;
  }
  .desc {
    text-decoration: justify;
  }
}
</style>
