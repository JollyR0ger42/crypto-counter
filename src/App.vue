<template>
<div class="wrapper">
  <div class="app">
    <div class="app__summary">
      Holdings:
      <span class="app__summary__holdings">{{holdings}} {{currency}}</span>
    </div>
    <app-select class="app__select" />
    <list-of-crypto />
  </div>
</div>
</template>

<script>
import AppSelect from './components/AppSelect.vue'
import ListOfCrypto from './components/ListOfCrypto.vue'

export default {
  components: {
    AppSelect,
    ListOfCrypto
  },

  data () {
    return {
      holdings: 100500,
      currency: 'USD'
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 20px;
  display: flex;
  justify-content: center;
  background-color: #000;
  width: 100%;
  height: 100%;
}

.app {
  max-width: 800px;
  display: flex;
  flex-direction: column;
  align-items: center;

  &__select {
    margin: 15px;
  }

  &__summary {
    font-size: 1.5rem;
    text-align: center;
    margin-bottom: 25px;

    &__holdings {
      color: #5AC706;
      font-size: 1.8rem;
    }
  }
}
</style>
