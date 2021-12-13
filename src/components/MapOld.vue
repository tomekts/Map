<template>
<div id='input'>

  </div>
  <div id="map-root" class="map">

   X<input type="text" v-model="x" />
   Y<input type="text" v-model="y" />

   <button id="go-it">przejdz do:</button>
   Miejscowość<input type="text" v-model="place" />
   <button id="check">spr:</button>
   <button v-on:click='showInMap'>Test</button>
  </div>

</template>
<script>

// import openlayer css for style
import 'ol/ol.css'
// This is library of openlayer for handle map
import Map from 'ol/Map'
import View from 'ol/View'
import { Tile as TileLayer, Vector as VectorLayer } from 'ol/layer'
import { OSM, Vector as VectorSource } from 'ol/source'
import GeoJSON from 'ol/format/GeoJSON'
import { fromLonLat, toLonLat } from 'ol/proj'
import axios from 'axios'

export default {
  data () {
    return {
      place: '',
      cord: null,
      x: 0,
      y: 0

    }
  },

  async mounted () {
    // await this.initiateMap();
    await this.newMap(this)
  },
  methods: {

    showInMap () {
      console.log('te')
    },

    async getPlaceCord (data, map) {
      try {
        const res = await axios.get('https://api.mapbox.com/geocoding/v5/mapbox.places/' + data.place + '.json?access_token=pk.eyJ1IjoidG9tZWt0cyIsImEiOiJja3d3MWQ2eXYyNWc5Mm9wbTA3d2QydmR4In0.ZZWcrSnMBj9Ywzk36xh2pg')
          .then(res => {
            this.cord = res.data.features[4].center
          })
      } catch (e) {
        console.log(e)
        alert('bład')
      }
    },

    newMap (data) {
      var map = new Map({
        target: 'map-root',
        projection: 'EPSG:27700',
        layers: [
          new TileLayer({
            source: new OSM()
          })
        ],
        view: new View({
          zoom: 1,
          // center:[da.x,da.y],
          center: fromLonLat([data.x, data.y]),
          constrainResolution: true
        })
      })

      document.getElementById('go-it').onclick = function () {
        var view = map.getView()
        var zoom = view.getZoom()
        var center = view.getCenter()
        view.animate({
        // center:[da.x,da.y],
          center: fromLonLat([data.x, data.y]),
          duration: 20
        })
      }
      document.getElementById('check').onclick = async () => {
        await this.getPlaceCord(this, map)
        if (data.cord) {
          var view = map.getView()
          // var center = view.getCenter();
          view.animate({
            center: fromLonLat([data.cord[0], data.cord[1]]),
            duration: 200
          })
        }
      }
    }

  }
}
</script>
<style>
.map {
        height: 400px;
        width: 100%;
      }
</style>
