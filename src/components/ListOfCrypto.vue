<template>
  <div>
    <crypto-card />
    <crypto-card />
    <crypto-card />
  </div>
</template>

<script>
import CryptoCard from './CryptoCard.vue'

export default {
  name: 'ListOfCrypto',

  components: {
    CryptoCard
  }
}
</script>
