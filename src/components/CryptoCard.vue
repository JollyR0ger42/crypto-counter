<template>
  <div class="crypto-card">
    <p>{{name}}</p>
    <p>Price:
      <span class="crypto-card__price">{{price}} {{currency}}</span>
    </p>
    <p>Holdings: {{holdings}}</p>
  </div>
</template>

<script>
export default {
  name: 'CryptoCard',

  data () {
    return {
      name: 'BTC',
      price: 45000,
      currency: 'USD',
      holdings: 100
    }
  }
}
</script>

<style lang="scss" scoped>
.crypto-card {
  display: inline-block;
  padding: 10px;
  margin: 10px;
  border: 2px solid #fff;
  font-size: 1rem;

  &__price {
    color: #B2D806;
  }
}
</style>
