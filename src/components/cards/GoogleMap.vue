<template>
  <div class='GoogleMap' style="width: 75vw; height: 58vh;"/>
</template>

<script>
import gmapsInit from '../../utils/gmaps';
import { locations } from "../../assets/data/campusLocations.json"

export default {
  name: 'GoogleMap',
  async mounted() {
    try {
      const google = await gmapsInit();
      //const geocoder = new google.maps.Geocoder();
      const map = new google.maps.Map(this.$el);      

      map.setCenter({ lat: 53.467249, lng: -2.234202});
      map.setZoom(17);

      const markerClickHandler = (marker) => {
        map.setZoom(18);
        map.setCenter(marker.getPosition());        
        this.sendMarker(marker.name);          
      };

      const mapClickHandler = () => {
        map.setZoom(17);
        map.setCenter({ lat: 53.467249, lng: -2.234202});
        this.$props.selectedSpace.text = "";
      }

      const markers = locations.map((location) => {
          const marker = new google.maps.Marker({ ...location, map });
          marker.addListener(`click`, () => markerClickHandler(marker));
          return marker;
      });

      map.addListener(`click`, () => mapClickHandler());
           
    } catch (error) {
      console.error(error);
    }
  },

  methods: {
    sendMarker(marker) {
      console.log(marker);                  
      this.$props.selectedSpace.text = marker;
    }
  },

  props: {
    selectedSpace: {
      type: Object,
      required: true
    }
  },
  
};
</script>
