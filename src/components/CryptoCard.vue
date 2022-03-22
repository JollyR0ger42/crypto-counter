<template>
  <div class="crypto-card">
    <p>{{name}}</p>
    <p>Price:
      <span class="crypto-card__price">{{price}} {{currency}}</span>
    </p>
    <p>Amount: 
      <input size="10" class="crypto-card__amount__input" v-model="amount">
    </p>
    <p>Holdings:
      <span class="crypto-card__price">100 USD</span>
    </p>
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
      amount: 100
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

  &__amount {
    &__input {
      font-size: 1rem;
      background-color: rgba(0,0,0,0);
    }
  }

  &__price {
    color: #B2D806;
  }
}
</style>
