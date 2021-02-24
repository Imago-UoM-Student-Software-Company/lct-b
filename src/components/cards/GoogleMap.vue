<template>
  <div class="GoogleMap" style="width: 75vw; height: 58vh;"/>
</template>

<script>
import gmapsInit from '../../utils/gmaps';

export default {
  name: 'GoogleMap',
  async mounted() {
    try {
      const google = await gmapsInit();
      //const geocoder = new google.maps.Geocoder();
      const map = new google.maps.Map(this.$el);      

      map.setCenter({ lat: 53.467249, lng: -2.234202});
           
    } catch (error) {
      console.error(error);
    }
  },
};
</script>
