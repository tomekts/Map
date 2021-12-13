<template>
  <div class="w-full">
      <ShowMap :cordinate="cordinate" @inputPlace="SearchPlace"/>
      <LeaftletMap/>
  </div>
</template>

<script>

import ShowMap from '../components/ShowMap.vue'
import LeaftletMap from '../components/LeaftletMap.vue'
export default {

  name: 'Map',
  components: {
    ShowMap, LeaftletMap
  },
  data () {
    return {
      cordinate: [20, 51]
    }
  },
  methods: {

    SearchPlace (cord) {
      this.cordinate = cord
    }
  }

}
</script>

<style>

</style>
