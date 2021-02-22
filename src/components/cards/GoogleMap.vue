<template>
    <div>     
        <GmapMap
          :center=center
          :zoom="17"
          map-type-id="roadmap"
          style="width: 1270px; height: 410px"
        >
          <GmapMarker
            :key="index"
            v-for="(m, index) in markers"
            :position="m.position"
            :clickable="true"            
            @click="center = m.position"
          />
        </GmapMap>
    </div>
</template>

<script>

const kilburn = { lat: 53.467249, lng: -2.234202};
const university = { lat: 53.46675, lng: -2.23388};

export default {
  name: "GoogleMap",
  data() {
    return {
      markers: [
        {
          position: kilburn,
          name: "Kilburn Building"
        },
        {
          position: university,
          name: "University Place"

        },
      ],
      center: kilburn,
    }
  },
  methods: {    
    clearMap() {
      this.markers = []
    },
  }
};
</script>

<style scoped></style>