<template>
<div>
  <v-btn
  elevation="2"
>Save</v-btn>
  <l-map style="height: 550px" :zoom="zoom" :center="center" >
    
    <l-tile-layer :url="url" :attribution="attribution">
    </l-tile-layer>
    <l-geo-json :geojson="geojson" @click="handleClick">       
    </l-geo-json>

    <l-marker  :visible=makerOn :lat-lng="marker" ref="marker">
      <l-popup class="popup" ref="markers" >
        <div class="popupContent">
          <div class="popupContent__inputs">
            <div class="popupContent__inputs--row">
              <label for="">Muestras:</label>
              <input type="text" placeholder="1"/>
            </div>
            <div class="popupContent__inputs--row">
            <label for="">Radio (m) </label>
            <input type="text" v-model=radio>
            </div>
          </div>
          <div class="popupContent__img">
              <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-trash" width="72" height="72" viewBox="0 0 24 24" stroke-width="1.5" stroke="#000000" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <line x1="4" y1="7" x2="20" y2="7" />
                <line x1="10" y1="11" x2="10" y2="17" />
                <line x1="14" y1="11" x2="14" y2="17" />
                <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
                <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
              </svg>
          </div>
        </div>
      </l-popup>
    </l-marker>
      <l-circle-marker :visible=makerOn
          :lat-lng="marker"
          :radius=radio
          color="red"
        />



    <l-marker :visible=true v-for="marker, index in marker2" :lat-lng="marker" :key="index" >
      <l-popup class="popup" ref="markers" >
        <div class="popupContent">
          <div class="popupContent__inputs">
            <div class="popupContent__inputs--row">
              <label for="">Muestras:</label>
              <input type="text" placeholder="1"/>
            </div>
            <div class="popupContent__inputs--row">
            <label for="">Radio (m) </label>
            <input type="text" v-model=radio>
            </div>
          </div>
          <div class="popupContent__img">
              <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-trash" width="72" height="72" viewBox="0 0 24 24" stroke-width="1.5" stroke="#000000" fill="none" stroke-linecap="round" stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none"/>
                <line x1="4" y1="7" x2="20" y2="7" />
                <line x1="10" y1="11" x2="10" y2="17" />
                <line x1="14" y1="11" x2="14" y2="17" />
                <path d="M5 7l1 12a2 2 0 0 0 2 2h8a2 2 0 0 0 2 -2l1 -12" />
                <path d="M9 7v-3a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v3" />
              </svg>
          </div>
        </div>
      </l-popup>
    </l-marker>

    <l-circle-marker v-for="marker, index in marker2" :visible=makerOn :key="index"
          :lat-lng="marker"
          :radius=radio
          color="red"
        />

   <!--  <l-marker  :visible=true  :lat-lng="marker" ref="marker" v-for="maker2 in makers" :key="makers.lat">
      
    </l-marker> -->
</l-map>
</div>
</template>
<style lang="scss">
    .leaflet-popup-content-wrapper, .leaflet-popup.tip {
      background: #ddd;
      border:1px solid rgb(88, 87, 87);    
      }
    .popupContent{
      display: flex;  
    }
    .popupContent__inputs{
      width:80%;
      display: flex;
      flex-direction: column;
      justify-content: space-around;
    }
    .popupContent__inputs--row{
      display: flex;
      justify-content: space-around;
      align-items:center;
      height: 30px;
      label{
        font-size: 11px;
      }
      input{
        width: 60px;
        background: white;
        margin:0 8px;
        height: 25px;
        font-size: 13px;
        padding:2px;
        border-radius: 6px;
        border:1px solid rgb(88, 87, 87);    
        text-align: center;
      }
    }
    .leaflet-popup-content{
      margin:8px;

    }
    .popupContent__img{
      width:20%;
      display: flex;
      justify-content: center;
      align-items: center;
      border-left: 1px solid rgb(88, 87, 87);
    }
    .popupContent__img svg{
      width:30px;
      height: 30px;
    }
    .popupContent__img svg:hover{
      transform: scale(1.2);
      cursor: pointer;
      animation-duration: 400ms;
    }

</style>

<script>
import {LMap, LMarker,LPopup, LTileLayer, LGeoJson, LCircleMarker} from 'vue2-leaflet';
import MakerCustom from '../components/Maker.vue'

export default {
  methods: {
    handleClick (e) {            
      this.marker= [e.latlng.lat,e.latlng.lng],
      this.makerOn = true
       this.$nextTick(() => {
          this.$refs.marker.mapObject.openPopup()
      })
      this.marker2.push(this.marker)
      console.log(this.marker2)
    },

  },
  components: {
    LMap,
    LTileLayer,
    LGeoJson,
    LMarker,
    LPopup,
    LCircleMarker,
    MakerCustom
    
  },
  data () {
    return {
      url:"http://mt0.google.com/vt/lyrs=s&x={x}&y={y}&z={z}",
      
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 20,
      center: [-14.010508830892729,-75.80318792584497],
      geojson: null,
      marker2: [],    
      marker: [-14.010508830892729,-75.80318792584497],
      makerOn:false,
      lat:0,
      long:0,
      radio:8

      };
  },
  mounted(){
    /* this.$nextTick(() => {
          this.$refs.marker.mapObject.openPopup()
      })  */
  },
  async created () {
    const response =  require('../initialState.json')

    this.geojson = await response.features;
  }
}
</script>
