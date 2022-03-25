<template>
  <div class="crypto-card">
    <button
      class="crypto-card__close"
      @click="$emit('remove-crypto')"
    >x</button>
    <p class="crypto-card__title">{{label}}</p>
    <p>Price:
      <span class="crypto-card__price">{{price || '-'}} {{currency}}</span>
    </p>
    <div class="crypto-card__amount">
      <span class="crypto-card__amount__label">Amount:</span>
      <input
        class="crypto-card__amount__input"
        type="number"
        :value="_amount"
        @input="handleAmount"
        size="10"
      >
    </div>
    <p>Holdings:
      <span class="crypto-card__holdings">{{holdings || '-'}} {{currency}}</span>
    </p>
  </div>
</template>

<script>
export default {
  name: 'CryptoCard',

  props: {
    symbol: String,
    label: String,
    amount: Number,
    currency: String,
    price: Number,
    holdings: Number,
  },

  emits: {
    'remove-crypto': null,
    'set-crypto-price': Number
  },

  data () {
    return {
      _amount: this.amount
    }
  },

  methods: {
    handleAmount (event) {
      this._amount = Number(event.target.value)
      this.$emit('set-crypto-price', this._amount)
    }
  }
}
</script>

<style lang="scss" scoped>
.crypto-card {
  position: relative;
  display: inline-block;
  padding: 10px;
  margin: 10px;
  border: 2px solid #fff;
  font-size: 1rem;

  &__close {
    box-sizing: border-box;
    position: absolute;
    top: 2px;
    right: 3px;
    background-color: #fff;
    font-size: 1rem;
    width: 1rem;
    height: 1rem;
    color: red;
    font-weight: bold;
    display: flex;
    align-items: center;
    justify-content: center;
    border: solid 1.5px #664cc3;
  }

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
    color: #E0CE07;
  }

  &__holdings {
    color: #B2D806;
  }
}
</style>
