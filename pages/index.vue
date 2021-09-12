<template>
<div>
  <div class="header">
    <h2>Sampling Points</h2>
    <v-btn  color="primary"
    elevation="2" @click="handlePrint"
    >Guardar</v-btn>
  </div>
  <l-map width="100%" style="height: 80vh" :zoom="zoom" :center="center" >
    <l-control-zoom position="bottomright"  ></l-control-zoom>
    <l-tile-layer :url="url" :attribution="attribution">
    </l-tile-layer>
    <l-geo-json :geojson="geojson" @click="handleClick">       
    </l-geo-json>
    <l-marker  :visible=makerOn :lat-lng="marker" ref="marker" color="red">
      <l-popup class="popup" ref="markers" >
        <div class="popupContent">
          <div class="popupContent__inputs">
            <div class="popupContent__inputs--row">
              <label for="">Muestras:</label>
              <input type="number" v-model=num_samples>
            </div>
            <div class="popupContent__inputs--row">
            <label for="">Radio (m) </label>
            <input type="number" v-model=radio >
            </div>
          </div>
          <div class="popupContent__img" >
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
        
      <div v-for="marker, index in marker2" :key="index">
            <maker-custom :index="index" :handleDelete="handleDelete" 
              :radio= parseInt(marker.radio)
              :num_samples=marker.num_sample 
              :latlong=[marker.position.latitude,marker.position.longuitude]>
            </maker-custom>
      </div>
    
</l-map>
</div>
</template>
<style lang="scss">
      .header{
      display: flex;
      align-items: center;
      justify-content: space-between;
        h2{
          text-transform: uppercase;
        }
      }
    .leaflet-popup-content-wrapper, .leaflet-popup.tip {
      background: #ddd;
      border:1px solid rgb(88, 87, 87);    
      }
    .popupContent{
      display: flex;  
        &__inputs{
          width:80%;
          display: flex;
          flex-direction: column;
          justify-content: space-around;
          &--row{
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
        }
        &__img{
            width:20%;
            display: flex;
            justify-content: center;
            align-items: center;
            border-left: 1px solid rgb(88, 87, 87);
            svg{
                width:30px;
                height: 30px;
                filter: opacity(0.3);
            }
        }
    }
   
    .leaflet-popup-content{
      margin:8px;

    }
    
    
</style>

<script>
import {LMap, LMarker,LPopup, LTileLayer, LGeoJson, LCircleMarker} from 'vue2-leaflet';
import MakerCustom from '../components/Maker.vue'

export default {
  methods: {   
    handleDelete(id){      
      this.marker2.splice(id,1)
    },

    handlePrint(){      
     this.datos = { // esto representa 1 marcador
      "position": {
      "latitude": this.lat,
      "longuitude": this.long
    },
      "radio": this.radio, 
      "num_samples": this.num_samples // número de muestras
    }
    this.marker2.push(this.datos)   
    console.log(this.marker2)     
    },
    resetInputs(){
      this.radio=8
      this.num_samples=1
      this.lat=0  
      this.long=0
    },
    handleClick (e) {            
      this.marker= [e.latlng.lat,e.latlng.lng]
      this.resetInputs();
      this.lat = e.latlng.lat
      this.long = e.latlng.lng      
      this.makerOn = true
      this.$nextTick(() => {
         this.$refs.marker.mapObject.openPopup()
      })
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
      center: [-14.009659270463652,-75.80318792584497],
      geojson: null,
      marker2: [],    
      marker: [-14.010508830892729,-75.80288743973598],
      makerOn:false,
      lat:0,
      long:0,
      radio:8,
      num_samples:1,
      datos:{}
      };
  },
  mounted(){
   
  },
  async created () {
    const response =  require('../initialState.json')
    this.geojson = await response.features;
  }
}
</script>
