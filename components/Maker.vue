<template>
    <div>
    <l-marker  :visible=true :lat-lng="latlong"  >
      <l-popup class="popup" ref="markers" >
        <div class="popupContent">
          <div class="popupContent__inputs">
            <div class="popupContent__inputs--row">
              <label for="">Muestras:</label>
              <input type="text" placeholder="1"/>
            </div>
            <div class="popupContent__inputs--row">
            <label for="">Radio (m) </label>
            <input type="text" v-model="radio">
            </div>
          </div>
          <div class="popupContent__img" v-on:click="handleDelete(index)">
              <svg  xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-trash" width="72" height="72" viewBox="0 0 24 24" stroke-width="1.5" stroke="#000000" fill="none" stroke-linecap="round" stroke-linejoin="round">
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
      <l-icon
          iconUrl= 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-red.png'
          shadowUrl= 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png' 
          >
    </l-icon>
    </l-marker>

    <l-circle-marker :visible=true 
          :lat-lng="latlong"
          :radius=radio
          color="white"
        />
    </div>  
</template>
<style lang="scss" scoped>
.popupContent__img svg{
      filter: opacity(1);
}
.popupContent__img svg:hover{
      transform: scale(1.2);
      cursor: pointer;
      animation-duration: 400ms; 
      
    }
   
</style>

<script>
import { LMarker,LPopup, LCircleMarker} from 'vue2-leaflet';

export default {
  props: ["latlong", "radio", "handleDelete", "num_samples","index"],
  methods: {    
      handleDelete(id){
          this.$dispatch('handleDelete', id);
      }
  },
  components: {    
    LMarker,
    LPopup,
    LCircleMarker
    
  },
  data () {
    return {                 
      
      };
  },
  mounted(){
   
  },
  
}
</script>

