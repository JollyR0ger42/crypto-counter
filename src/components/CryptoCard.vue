<template>
  <div class="crypto-card">
    <p class="crypto-card__title">{{name}}</p>
    <p>Price:
      <span class="crypto-card__price">{{price}} {{currency}}</span>
    </p>
    <div class="crypto-card__amount">
      <span class="crypto-card__amount__label">Amount:</span>
      <input size="10" class="crypto-card__amount__input" v-model="amount">
    </div>
    <p>Holdings:
      <span class="crypto-card__price">100500000 USD</span>
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

  & > *:not(:last-child) {
    margin-bottom: 5px;
  }

  &__title {
    font-weight: bold;
    font-size: 1.1rem;
    margin-bottom: 10px;
  }

  &__amount {
    width: 100%;
    display: flex;
    align-items: center;
    flex-wrap: wrap;

    &__label {
      flex: 0 1;
    }

    &__input {
      flex: 1 2;
      width: 100%;
      min-width: 100px;
      margin-left: 5px;
      font-size: 1rem;
      background-color: rgba(0,0,0,0);
    }
  }

  &__price {
    color: #B2D806;
  }
}
</style>
