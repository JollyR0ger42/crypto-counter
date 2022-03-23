<template>
<div class="wrapper">
  <div class="app">
    <div class="app__summary">
      Holdings:
      <span class="app__summary__holdings">{{holdings}} {{currency}}</span>
    </div>
    <app-select
      class="app__select"
      :options="selectOptions"
      :refetching="true"
      @refresh-select-list="refetchCryptoList"
    />
    <crypto-list
      :selectedCrypto="selectedCrypto"
    />
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
      currency: 'USD',
      selectOptions: [
        {
          code: 'BTCUSDT',
          label: 'BTC / USDT'
        },
        {
          code: 'ETHBTC',
          label: 'ETH / BTC'
        },
        {
          code: 'DOGEETH',
          label: 'DOGE / ETH'
        }
      ],
      selectedCrypto: [{
          name: 'BTC',
          price: 45000,
          currency: 'USD',
          amount: 100
        }, {
          name: 'BTC',
          price: 45000,
          currency: 'USD',
          amount: 100
        }, {
          name: 'BTC',
          price: 45000,
          currency: 'USD',
          amount: 100
        }
      ]
    }
  },

  mounted () {
    window.document.title = this.holdings + this.currency
  },

  methods: {
    refetchCryptoList () {
      console.log('refetching')
    }
  }
}
</script>

<style lang="scss">
@import 'vue-select/dist/vue-select.css';

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
