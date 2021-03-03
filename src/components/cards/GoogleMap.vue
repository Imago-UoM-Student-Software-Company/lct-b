<template>
  <div class='GoogleMap' style="width: 75vw; height: 360px;"/>
</template>

<script>
import gmapsInit from '../../utils/gmaps';
import { data } from "../../assets/data/sistersBusiness.json";


export default {
  name: 'GoogleMap',
  async mounted() {
    try {
        const google = await gmapsInit();        
        const map = new google.maps.Map(this.$el);   
        const defaultZoom = 16;   
        map.setCenter({ lat: 53.467249, lng: -2.234202});
        map.setZoom(17);

        const markerClickHandler = (marker) => {
          map.setZoom(Math.min(map.getZoom() + 1), defaultZoom + 1);
          map.setCenter(marker.getPosition());        
          this.sendMarker(marker.ID);          
        };

        const mapClickHandler = () => {
          map.setZoom(Math.max(map.getZoom() - 1), defaultZoom - 1);
          map.setCenter({ lat: 53.467249, lng: -2.234202});
          this.$props.selectedSpace.text = "";              
        };
        
        const markers = this.createMarkers(data, map, google, markerClickHandler);

        map.addListener(`click`, () => mapClickHandler());
        this.$root.$on('selected-new-category', () => {  
          map.setZoom(Math.max(map.getZoom() - 1, defaultZoom - 1));
          this.updateMarkers(markers);
        });
                    
    } catch (error) {
        console.error(error);
      }
  },

  methods: {
    sendMarker(marker) {
      console.log(marker);                  
      this.$props.selectedSpace.text = marker;
    },
    createMarkers(data, map, google, markerClickHandler) {      
      const markers = data.map((location) => {                       
        const marker = new google.maps.Marker({ ...location, map });
        marker.addListener(`click`, () => markerClickHandler(marker));
        if(this.$props.categorySelected == marker.NAME)
          marker.setVisible(true);
        else
          marker.setVisible(false);
        return marker;
      });    

      return markers;                        
    },
    updateMarkers(markers) {
      markers.forEach(marker => {
        if(this.$props.categorySelected == marker.NAME)
          marker.setVisible(true);
        else
          marker.setVisible(false);
      });
    }    
  },

  props: {
    selectedSpace: {
      type: Object,
      required: true
    },
    categorySelected: {
      type: String,
      required: true
    }   
  },

  data() {
    return {
      markers: []
    }
  }
  
};
</script>
