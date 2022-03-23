<template>
  <div class="crypto-list">
    <crypto-card
      v-for="(crypto, idx) in selectedCrypto"
      v-bind="crypto"
      :key="idx"
      class="crypto-list__card"
    />
  </div>
</template>

<script>
import CryptoCard from './CryptoCard.vue'

export default {
  name: 'ListOfCrypto',

  components: {
    CryptoCard
  },

  props: {
    selectedCrypto: Array
  }
}
</script>

<style lang="scss" scoped>
.crypto-list {
  display: flex;
  flex-wrap: wrap;

  &__card {
    min-width: 250px;
  }

  @media (max-width: 600px) {
    flex-direction: column;
    align-items: center;
  }
}
</style>
