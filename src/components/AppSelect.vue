<template>
<div class="app-select">
  <v-select
    class="app-select__vselect"
    placeholder="Add crypto"
    :options="options"
    v-model="selected"
    @close="onInput"
  />
  <button
    @click="$emit('refresh-select-list')"
    class="app-select__refresh">
  {{refreshButtonText}}</button>
</div>
</template>

<script>
import VSelect from 'vue-select'

export default {
  name: 'AppSelect',

  components: {
    VSelect
  },

  props: {
    options: Array,
    refreshing: Boolean
  },

  emits: {
    'refresh-select-list': null,
    'select-crypto': null
  },

  data () {
    return {
      selected: null,
      refreshButtonText: 'Refresh',
      intervalId: null
    }
  },

  watch: {
    refreshing: {
      handler (newVal) {
        if (newVal) {
          this.refreshButtonText = '.'
          this.intervalId = setInterval(() => {
            if (this.refreshButtonText.length > 2)
              this.refreshButtonText = '.'
            else
              this.refreshButtonText += '.'
          }, 333)
        } else {
          clearInterval(this.intervalId)
          this.refreshButtonText = 'Refresh'
        }
      },
      immediate: true
    }
  },

  unmounted () {
    clearInterval(this.intervalId)
  },

  methods: {
    onInput () {
      if (this.selected?.code === 'Refresh') this.$emit('refresh-select-list')
      else this.$emit('select-crypto', {...this.selected, amount: 0})
      this.selected = null
    }
  }
}
</script>

<style lang="scss" scoped>
.app-select {
  width: 100%;
  max-width: 500px;
  display: flex;

  &__refresh {
    border: solid 1.5px #664cc3;
    border-radius: 2px;
    padding: 5px;
    margin-left: 5px;
  }

  &__vselect {
    flex: 1 0;
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
  
  }
}

* {
  text-align: center;
  background-color: #000;
  font-size: 1rem;
}
</style>
