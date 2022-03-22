<template>
  <v-select 
    class="app-select"
    placeholder="Add crypto"
    :options="['AAA', 'AAB', 'ABB']"
    v-model="selected"
    @close="onInput"
  />
</template>

<script>
import VSelect from 'vue-select'

export default {
  name: 'AppSelect',

  components: {
    VSelect
  },

  data () {
    return {
      selected: null
    }
  },

  methods: {
    onInput () {
      console.log(this.selected)
      this.selected = null
    }
  }
}
</script>

<style scoped>
.app-select {
  --vs-controls-color: #664cc3;
  --vs-border-color: #664cc3;

  --vs-dropdown-bg: #282c34;
  --vs-dropdown-color: #cc99cd;
  --vs-dropdown-option-color: #cc99cd;

  --vs-selected-bg: #664cc3;
  --vs-selected-color: #eeeeee;

  --vs-search-input-color: #eeeeee;

  --vs-dropdown-option--active-bg: #664cc3;
  --vs-dropdown-option--active-color: #eeeeee;

  width: 100%;
  max-width: 500px;
}

* {
  text-align: center;
  background-color: #000;
  font-size: 1rem;
}
</style>
