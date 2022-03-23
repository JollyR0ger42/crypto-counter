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
      :refreshing="refreshingSelect"
      @refresh-select-list="refetchCryptoList"
      @add-crypto="addCrypto"
    />
    <crypto-list
      :currency="currency"
      :selectedCrypto="selectedCrypto"
      @remove-crypto="removeCrypto"
    />
  </div>
</div>
</template>

<script>
import AppSelect from './components/AppSelect.vue'
import CryptoList from './components/CryptoList.vue'
import {ref} from 'vue'

export default {
  components: {
    AppSelect,
    CryptoList
  },

  setup () {
    let selectOptions
    try {selectOptions = JSON.parse(localStorage.getItem('selectOptions'))}
    catch {localStorage.removeItem('selectOptions')}
    finally {
      if (!selectOptions?.length) {
        selectOptions = [{ code: 'Refresh', label: 'Refresh crypto list.' }]
        localStorage.setItem('selectOptions', JSON.stringify(selectOptions))
      }
    }

    let selectedCrypto
    try {selectedCrypto = JSON.parse(localStorage.getItem('selectedCrypto'))}
    catch {localStorage.removeItem('selectedCrypto')}
  
    return {
      selectOptions: ref(selectOptions),
      selectedCrypto: ref(selectedCrypto ?? [])
    }
  },

  data () {
    return {
      apiUrl: 'https://api.binance.com/api/v3/',
      holdings: 100500,
      currency: 'USDT',
      refreshingSelect: false
    }
  },

  mounted () {
    window.document.title = this.holdings + this.currency
  },

  methods: {
    refetchCryptoList () {
      this.refreshingSelect = true
      fetch(this.apiUrl + 'exchangeInfo')
        .then(response => response.json())
        .then(data => {
          this.selectOptions = this.parseSymbols(data?.symbols)
          localStorage.setItem('selectOptions', JSON.stringify(this.selectOptions))
        })
        .finally(() => this.refreshingSelect = false)
    },
    parseSymbols (symbols) {
      const result = []
      symbols?.forEach(symbol => {
        if (symbol.quoteAsset === this.currency) {
          const listElement = {
            symbol: symbol.symbol,
            label: `${symbol.baseAsset}`
          }
          result.push(listElement)
        }
      })
      return result
    },
    addCrypto (crypto) {
      this.selectedCrypto.push(crypto)
      localStorage.setItem('selectedCrypto', JSON.stringify(this.selectedCrypto))
    },
    removeCrypto (idx) {
      this.selectedCrypto.splice(idx, 1)
      localStorage.setItem('selectedCrypto', JSON.stringify(this.selectedCrypto))
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
  overflow-y: scroll;
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
