<template>
  <div class="flex flex-col items-center justify-center h-screen">
    <fieldset class="w-full max-w-md p-4">
      <legend>
        <img src="./assets/TheLogo.png" alt="TIG TRANSFERT Logo" />
      </legend>
      <div
        class="border-l-4 border-r-4 border-orange-500 p-3 rounded mb-6 shadow bg-gray-100 font-semibold text-green-900 tracking-wide text-lg"
      >
        <center>Convertiseur de devises</center>
      </div>

      <div
        class="flex flex-col border-l-4 border-r-4 border-b-4 border-orange-500 p-3 rounded mb-6 shadow bg-gray-100 font-semibold text-green-900 tracking-wide text-lg"
      >
        <div class="flex items-center border rounded-lg p-2 space-x-2">
          <input
            v-model="amount1"
            class="w-24 p-1 rounded-md border"
            type="number"
            min="0"
            @input="convertCurrency"
          />
          <select
            v-model="currency1"
            class="rounded-md border-orange p-1 w-40"
            aria-label="Type de devise"
            @change="convertCurrency"
          >
            <option value="XOF">FCFA</option>
            <option value="AUD">Australien Dollar</option>
            <option value="EUR">Euro</option>
            <option value="YEN">Yen japonnais</option>
            <option value="RUB">Rouble Russie</option>
            <option value="CAD">Dollar canadien</option>
            <option value="USD">Dollar américain</option>
          </select>
        </div>
        <div class="flex items-center border rounded-lg p-2 space-x-2">
          <input
            v-model="amount2"
            class="w-24 p-1 rounded-md border"
            type="number"
            @input="convertCurrency"
          />
          <select
            v-model="currency2"
            class="rounded-md border p-1 w-40"
            aria-label="Type de devise"
            @change="convertCurrency"
          >
            <option value="XOF">FCFA</option>
            <option value="AUD">Australien Dollar</option>
            <option value="EUR">Euro</option>
            <option value="YEN">Yen japonnais</option>
            <option value="RUB">Rouble Russie</option>
            <option value="CAD">Dollar canadien</option>
            <option value="USD">Dollar américain</option>
          </select>
        </div>
      </div>
    </fieldset>
    <div class="flex justify-end items-center absolute bottom-0 right-0 w-16 h-16 mr-4 mb-4">
      <a aria-label="Chat on WhatsApp" href="https://wa.me/+22997616373" target="_blank"
        ><img alt="Chat on WhatsApp" src="./assets/logo-whatsapp.png" />
      </a>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'CurrencyConverter',
  data() {
    return {
      amount1: '',
      currency1: 'XOF',
      amount2: '',
      currency2: 'USD',
      exchangeRates: {}
    }
  },
  mounted() {
    this.fetchExchangeRates()
  },
  methods: {
    async fetchExchangeRates() {
      const response = await axios.get('https://cdn.taux.live/api/latest.json')
      this.exchangeRates = response.data.rates
    },
    async convertCurrency() {
      const rate = this.exchangeRates[this.currency2] / this.exchangeRates[this.currency1]
      this.amount2 = this.amount1 * rate
    }
  }
}
</script>
<style scoped>
.flex {
  display: flex;
}

.flex-col {
  flex-direction: column;
}

.items-center {
  align-items: center;
}

.justify-center {
  justify-content: center;
}

.h-screen {
  height: 100vh;
}

.w-full {
  width: 100%;
}

.max-w-md {
  max-width: 540px;
}

.p-4 {
  padding: 1rem;
}

select {
  width: 100%;
  border: 1px solid #ccc;
  padding: 0.5rem;
}

input {
  width: 100%;
  border: 1px solid #ccc;
  padding: 0.5rem;
}
</style>
