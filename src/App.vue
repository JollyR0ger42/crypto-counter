<template>
<div class="wrapper">
  <div class="app">
    <div class="app__summary">
      Holdings:
      <span class="app__summary__holdings">{{holdings}} {{currency}}</span>
    </div>
    <app-select class="app__select" />
    <crypto-list />
  </div>
</div>
</template>

<script>
import AppSelect from './components/AppSelect.vue'
import CryptoList from './components/CryptoList.vue'

export default {
  components: {
    AppSelect,
    CryptoList
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
  max-width: 900px;
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
      color: #59C606;
      font-size: 1.8rem;
    }
  }
}
</style>
