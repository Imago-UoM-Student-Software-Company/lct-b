<template>
  <div class='GoogleMap' style="width: 75vw; height: 58vh;"/>
</template>

<script>
import gmapsInit from '../../utils/gmaps';

const locations = [
   {
     position: {lat: 53.467249, lng: -2.234202},
     name: "Kilburn Building"              
   },
   {
     position: {lat: 53.46675, lng: -2.23388},
     name: "University Place"        
   },
];


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
      };

      const markers = locations.map((location) => {
          const marker = new google.maps.Marker({ ...location, map });
          marker.addListener(`click`, () => markerClickHandler(marker));
          return marker;
      });
           
    } catch (error) {
      console.error(error);
    }
  }
  
};
</script>
