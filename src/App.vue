<template>
<div class="wrapper">
  <div class="app">
    <div class="app__summary">
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
      @set-crypto-price="setCryptoAmount"
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
      currency: 'USDT',
      refreshingSelect: false,
      refreshTime: localStorage.getItem('refreshTime') || 60,
      refreshId: null
    }
  },

  computed: {
    holdings () {
      return this.selectedCrypto.reduce((acc, val) => acc + val.holdings, 0).toFixed(2)
    }
  },

  watch: {
    holdings () {
      window.document.title = this.holdings + ' ' + this.currency
    }
  },

  mounted () {
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
      this.refreshCrypto()
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
      this.selectedCrypto = [...this.selectedCrypto, crypto]
      localStorage.setItem('selectedCrypto', JSON.stringify(this.selectedCrypto))
      this.refreshCrypto()
    },
    removeCrypto (idx) {
      this.selectedCrypto.splice(idx, 1)
      this.selectedCrypto = [...this.selectedCrypto]
      localStorage.setItem('selectedCrypto', JSON.stringify(this.selectedCrypto))
    },
    refreshCrypto() {
      this.selectedCrypto.forEach(crypto => {
        fetch(this.apiUrl + 'avgPrice?symbol=' + crypto.symbol)
          .then(response => response.json())
          .then(data => crypto.price = +data.price)
          .finally(() => crypto.holdings = +(crypto.amount * crypto.price).toFixed(2))
      })
    },
    validateRefreshTime (event) {
      const newVal = event.target.value
      if (newVal === 0) this.refreshTime = 0
      else if (newVal < 10) this.refreshTime = 10
      else this.refreshTime = newVal
      localStorage.setItem('refreshTime', this.refreshTime)
      this.setRefreshInterval()
    },
    setRefreshInterval () {
      clearInterval(this.refreshId)
      this.refreshCrypto()
      if (this.refreshTime) {
        this.refreshId = setInterval(this.refreshCrypto, this.refreshTime * 1000)
      }
    },
    setCryptoAmount (payload) {
      const {val, idx} = payload
      this.selectedCrypto[idx].amount = val
      this.$forceUpdate()
      localStorage.setItem('selectedCrypto', JSON.stringify(this.selectedCrypto))
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
  padding: 0 10px;
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
    margin-bottom: 15px;

    &__holdings {
      color: #59C606;
      word-break: break-word;
    }
  }
}
</style>
