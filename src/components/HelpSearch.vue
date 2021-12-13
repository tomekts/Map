<template>
   <div class="wrapper inline-block justify-center max-w-xs   ">
    <input
    v-model="inputValue"
    placeholder="Podaj miejsce"
    class="border-2 w-64 border-gray-400 text-left p-2 h-8  text-l "
    @input="handleChange"
    @mouseenter="showList"
    />

    <ul id="listPlace" >
        <li @mouseenter="enterMouse(index)" @click="searchPlace(index)" v-for="(item, index) in place" v-bind:key="index" class="text-left pl-2 bg-white">
            <p class="text-lg">{{item.label.split(',')[0]}}</p>
            <p class="text-sm">{{item.label.split(',')[1]}},
            {{item.label.split(',')[2]}},</p>
            <p class="text-xs">{{item.label.split(',')[3]}}</p>

        </li >
    </ul>
  </div>
</template>

<script>

import axios from 'axios'
import _ from 'lodash'

export default {

  data () {
    return {
      place: [],
      inputValue: ''
    }
  },
  emits: ['inputPlace'],

  methods: {
    // ukazuje listre podpowiedzi po najechaniu na pole input
    showList () {
      const list = document.getElementById('listPlace')
      list.style.display = 'block'
    },
    // przesyła koordynato z typem obiektu do componentu mapa w celu ustawienia na mapie
    searchPlace (e) {
      const cord = [
        this.place[e].point.coordinates[0],
        this.place[e].point.coordinates[1],
        this.place[e].type
      ]
      this.$parent.$emit('inputPlace', cord)
      const list = document.getElementById('listPlace')
      list.style.display = 'none'
    },

    // oczekuje okresloną ilosc ms i wykonuje metode zapytani
    handleChange: _.debounce(function (e) {
      console.log(e.target.value)
      if (e.target.value) {
        this.get(e.target.value)
      }
    }, 500),

    // zmienia wpis w polu input, po najechaniu na liste podpowiedzi
    enterMouse (e) {
      this.inputValue = this.place[e].label
    },

    // pobiera dane lokalizacji z api
    async get (query) {
      const key = 'b088a7c4-edad-474a-a13d-4cb2a9c6c8d0'
      try {
        await axios.get('https://address.search.api.ongeo.pl/1.0/autocomplete?api_key=' + key + '&query=' + query + '')
          .then(res => {
            console.log(res.data)
            this.place = res.data
          })
      } catch (e) {
        console.log(e)
        console.log('Bład zapytania')
      }
    }
  }
}
</script>

<style>
.wrapper ul :hover{
    background-color: rgb(240, 234, 234);
    /* transition: 0.2s linear; */
    cursor: pointer;

}
</style>
