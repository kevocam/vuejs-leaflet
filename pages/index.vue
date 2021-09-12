<template>

  <div>
    
    <l-map
      :zoom="zoom"
      :center="center"
      style="height: 500px; width: 100%"
    >
      <l-tile-layer
        :url="url"
        :attribution="attribution"
      />
      <l-geo-json
        v-if="show"
        :geojson="geojson"
        :options="options"
        :options-style="styleFunction"
      />
      <!-- <l-marker :lat-lng="marker" >
        <l-popup >
          <div class="popup" >
              <h1>sadsa</h1>

          </div>

        </l-popup>

      </l-marker> -->
              <l-circle-marker
              :lat-lng="center"
              :radius="5"
              :color="red"
            />
    </l-map>
  </div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LGeoJson, LPopup,LCircleMarker } from "vue2-leaflet";

export default {
  name: "Example",
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
    LMarker,
    LPopup,
    LCircleMarker
  },
  data() {
    return {
      loading: false,
      show: true,
      enableTooltip: true,
      zoom: 23,
      center: latLng(-14.010508830892729,-75.80318792584497 ),
      geojson: null,
      fillColor: "#e4ce7f",
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',      
      marker: latLng(-14.010508830892729,-75.80318792584497 )
    };
  },
  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction
      };
    },
    styleFunction() {
      const fillColor = this.fillColor; // important! need touch fillColor in computed for re-calculate when change fillColor
      return () => {
        return {
          weight: 1,
          color: "#ECEFF1",
          opacity: 1,
          fillColor: fillColor,
          fillOpacity: 1
        };
      };
    },
    onEachFeatureFunction() {
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.bindTooltip(
          "<div><input type='text' placeholder='asd'>:" +
            
            "</div><div>nom: " +
            feature.properties.nom +
            "</div>",
          { permanent: false, sticky: false }
        );
      };
    }
  },
  async created() {
    this.loading = false;
    let data= require('../initialState.json')
    console.log(data)
    
    
    this.geojson = data.features;
    this.loading = false;
  }
};
</script>
