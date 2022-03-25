<template>
  <div class="crypto-list">
    <crypto-card
      class="crypto-list__card"
      v-for="(crypto, idx) in selectedCrypto"
      v-bind="crypto"
      :currency="currency"
      :key="idx"
      @remove-crypto="$emit('remove-crypto', idx)"
      @set-crypto-price="val => $emit('set-crypto-price', {val, idx})"
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
    selectedCrypto: Array,
    currency: String
  }
}
</script>

<style lang="scss" scoped>
.crypto-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;

  &__card {
    min-width: 200px;
  }

  @media (max-width: 600px) {
    flex-direction: column;
    align-items: center;
  }
}
</style>
