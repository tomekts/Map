<template>
    <div class="grid">
        <!-- <input
        class="border-2 border-gray-400 text-center h-8  text-l "
        id='place'
        placeholder="Podaj miejsce"
        @input="handleChange"
        autocomplete="off"
        />    -->
        <HelpSearch />
    </div>

</template>

<script>
import HelpSearch from '@/components/HelpSearch'
import _ from 'lodash'

export default {
  components: {
    HelpSearch
  },

  name: 'Searchinput',
  emits: ['inputPlace'],

  methods: {
    handleChange: _.debounce(function (e) {
      if (e.target.value) {
        this.$parent.$emit('inputPlace', e.target.value)
      }
    }, 500)
  }

}
</script>

<style>

</style>
