<template>
    <div class="map_container top-0 ">
        <h1 v-if="!loadMap" >Wyszukaj na mapie</h1>

        <div v-if="loadMap" class="absolute w-full">
            <div class=" absolute right-0 z-10 w-64  "><HelpSearch/></div>
            <div  id="map-root" class="map z-0 relative  "> </div>
        </div>

        <img v-if="!loadMap" class="image cursor-pointer w-1/4 m-auto" src="../assets/image_map.png" @click="LoadMap">
    </div>
</template>

<script>
/* eslint-disable */
import 'ol/ol.css'
import Map from 'ol/Map'
import View from 'ol/View'
import { Tile as TileLayer } from 'ol/layer'
import { OSM } from 'ol/source'
import { fromLonLat } from 'ol/proj'
import _ from 'lodash'
import HelpSearch from './HelpSearch.vue'
export default {

  components: {
    HelpSearch
  },

  data () {
    return {
      loadMap: false
    }
  },
  props: {
    cordinate: Array
  },
  watch: {
    // oczekiwanie na załadowanie mapy
    loadMap: _.debounce(function (e) {
      this.Map()
    }, 50),
    // sprawdzanie zmieny lokaliazacji na mapie
    cordinate: function () {
      var view = this.map.getView()
      console.log(this.checkSize(this.cordinate[2]))
      view.animate({
        zoom: this.checkSize(this.cordinate[2]),
        center: fromLonLat([this.cordinate[0], this.cordinate[1]]),
        duration: 20
      })
    }
  },
  async mounted () {
    // await this.Map();
  },

  methods: {

    // uruchomienie mapy na stronie
    LoadMap () {
      this.loadMap = true
    },
    // generowanie mapy
    Map () {
      this.map = new Map({
        target: 'map-root',
        projection: 'EPSG:27700',
        layers: [
          new TileLayer({
            source: new OSM()
          })
        ],
        view: new View({
          zoom: 7,
          center: fromLonLat([this.cordinate[0], this.cordinate[1]]),
          constrainResolution: true
        })
      })
    },
    // sprawdzanie wielkości obiektu
    checkSize (size) {
      console.log(size)
      switch (size) {
        case 'city':
          return 14
          break
        case 'houseNumber':
          return 21
          break
        case 'street':
          return 18
          break
        default:
          break
      }
    }
  }
}
</script>

<style>
.map_container{
    overflow: hidden ;

}
.map {
        height: 400px;
        width: 100%;
        /* visibility: hidden; */
        position: absolute;
}

</style>
