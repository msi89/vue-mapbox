<template>
  <MglMarker
    :coordinates="[longitude, latitude]"
    :color="color"
    @click="onClickHandler"
  >
    <MglPopup>
      <slot />
    </MglPopup>
  </MglMarker>
</template>

<script>
import { MglMarker, MglPopup } from 'vue-mapbox'

export default {
  components: { MglMarker, MglPopup },
  props: {
    latitude: { type: Number, required: true },
    longitude: { type: Number, required: true },
    color: { type: String, default: 'lightblue' }
  },
  methods: {
    onClickHandler() {
      this.$emit('onMarkerClick', this.latitude, this.longitude)
    }
  }
}
</script>

<style></style>
