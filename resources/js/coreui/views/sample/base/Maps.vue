<template>
  <div class='row map'>
        <template slot="header">
      <slot name="caption">Table</slot>
    </template>
    <h2>Current Zoom : {{currentZoom}}, Current Center : {{currentCenter}} </h2>
    <l-map
      @update:zoom="zoomUpdate"
      @update:center="centerUpdate" 
      :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker 
      :key="index"
      v-for="(Gis,index) in Giss"
      :lat-lng="Position(Gis.latitude,Gis.longitude)"
    
      >

      </l-marker>
    </l-map>

  </div>
 
</template>

<script>
import { constants } from "crypto";
import { LMap, LTileLayer, LMarker, LIcon } from "vue2-leaflet";
const jpg1 = '../images/1.jpg';

export default {
  name: "CMaps",
  components: { LMap, LTileLayer, LMarker },
  data: () => {
    return {
      Giss:[],
      /*-----LEFLET-----*/
      zoom: 5,
      center: L.latLng(32.509762, -101.777344),
      currentZoom:3,
      currentCenter:L.latLng(47.41322, -1.219482),
      // url: "https://tile.thunderforest.com/transport/{z}/{x}/{y}.png?apikey=e955b6c9bc0b4ec6a38495e3f90d87b5",
      // url:'https://gitlab.com/IvanSanchez/Leaflet.GridLayer.GoogleMutant',
      url : "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      marker: L.latLng(47.41322, -1.219482),
      // normalIcon :[35,35],
      // largeIcon :[50,50],
      defaultIcon: L.icon({
        // iconUrl: 'http://leafletjs.com/examples/custom-icons/leaf-green.png',
        iconUrl: jpg1,
        iconSize:   [30,30],
        
      }),
      selectedIcon: L.icon({
        iconUrl: 'http://leafletjs.com/examples/custom-icons/leaf-red.png',
        shadowUrl: 'http://leafletjs.com/examples/custom-icons/leaf-shadow.png',
        // iconSize : [30,30]
      }),
    }
  },
  mounted() {
     axios
      .get('https://api.openbrewerydb.org/breweries')
      .then(response => (this.Giss = response.data)
      .map(r=>{
        r.iconSize = [25,15];
        r.iconUrl = 'http://leafletjs.com/examples/custom-icons/leaf-green.png';
        return r;
      }))
      // .then(response => (this.Giss = response.data.filter(response=>response.state=='Arizona')))
      .catch(error => console.log(error));
    
  },
  computed: {
    
  },
  watch:{
    Giss:{
      handler :function(val){
        console.log(val)
      },
      deep:true
    }
    
  },
  methods: {
    mouseOver(Gis,index){
        this.Giss[index].iconSize=[50,50];
          // this.Giss.splice(index,1);
          // console.log('size = '+this.Giss[index].iconSize+"  "+index)
        },
    Position(lat,long){
      return L.latLng(lat,long)
    },
    centerUpdate(center){ 
      return this.currentCenter=center
    },
    zoomUpdate(zoom){
      return this.currentZoom=zoom
    },
    klik(index){
      // this.Giss[index].iconSize=this.largeIcon
      // console.log(this.Giss[index].iconSize)
    },
    jnsIcon(Gis,index){
      if(Gis.state=='Arizona'){
        // this.Gis[index].iconSize = this.normalIcon
        return this.selectedIcon
      }else{
        //  this.Gis[index].iconSize = this.normalIcon
        return this.defaultIcon
      }
    }
  }
};
</script>
<style scoop>
    .map{
      height: 100%;
      width:100%;
    }
</style>
