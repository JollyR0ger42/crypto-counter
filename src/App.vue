<template>
<div class="wrapper">
  <div class="app">
    <div class="app__summary">
      Holdings:
      <span class="app__summary__holdings">{{holdings}} {{currency}}</span>
    </div>
    <div class="app__refresh">
      <span>Refresh (min 10 sec):</span>
      <input
        class="app__refresh__input"
        v-model="refreshTime"
        @change="validateRefreshTime"
        size="1"
      >
    </div>
    <app-select
      :options="selectOptions"
      :refreshing="refreshingSelect"
      @refresh-select-list="refetchCryptoList"
      @add-crypto="addCrypto"
    />
    <crypto-list
      :currency="currency"
      :selectedCrypto="selectedCrypto"
      @remove-crypto="removeCrypto"
      @set-crypto-price="setCryptoPrice"
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
      selectedCrypto: ref(selectedCrypto ?? []),
      selectOptions: ref(selectOptions),
    }
  },

  data () {
    return {
      apiUrl: 'https://api.binance.com/api/v3/',
      holdings: 100500,
      currency: 'USDT',
      refreshingSelect: false,
      refreshTime: 60,
      refreshId: null
    }
  },

  mounted () {
    window.document.title = this.holdings + this.currency
    this.setRefreshInterval()
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
      console.log('addCrypto', crypto)
      this.selectedCrypto = [...this.selectedCrypto, crypto]
      localStorage.setItem('selectedCrypto', JSON.stringify(this.selectedCrypto))
    },
    removeCrypto (idx) {
      this.selectedCrypto.splice(idx, 1)
      this.selectedCrypto = [...this.selectedCrypto]
      localStorage.setItem('selectedCrypto', JSON.stringify(this.selectedCrypto))
    },
    refreshCrypto() {
      console.log('crypto refreshed')
    },
    validateRefreshTime (event) {
      const newVal = event.target.value
      if (newVal === 0) this.refreshTime = 0
      else if (newVal < 10) this.refreshTime = 10
      else this.refreshTime = newVal
      this.setRefreshInterval()
    },
    setRefreshInterval () {
      clearInterval(this.refreshId)
      this.refreshCrypto()
      if (this.refreshTime) {
        console.log('new timer', this.refreshTime)
        this.refreshId = setInterval(this.refreshCrypto, this.refreshTime * 1000)
      }
    },
    setCryptoPrice (payload) {
      const {val, idx} = payload
      this.selectedCrypto[idx].amount = val
      this.$forceUpdate()
    }
  }
}
</script>

<style lang="scss">
@import 'vue-select/dist/vue-select.css';
@import './styles.css';

.wrapper {
  display: flex;
  justify-content: center;
  background-color: #000;
  width: 100%;
  height: 100%;
  overflow: overlay;
}

.app {
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;

  &__refresh {
    font-size: 1rem;

    &__input {
      background-color: #000;
      border: solid 1.5px #664cc3;
      font-size: 1rem;
      margin-left: 5px;
      border-radius: 2px;
      padding-left: 5px;
    }
  }

  &__summary {
    font-size: 1.5rem;
    text-align: center;
    margin-bottom: 25px;

    &__holdings {
      color: #59C606;
      font-size: 1.8rem;
      word-break: break-word;
    }
  }
}
</style>
