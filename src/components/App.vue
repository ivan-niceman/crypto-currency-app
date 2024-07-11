<script>
import InputSection from './InputSection.vue'
import CryptoList from './CryptoList.vue'
import CryptoConvert from 'crypto-convert'

const convert = new CryptoConvert()

export default {
  components: { InputSection, CryptoList },
  data() {
    return {
      amount: 0,
      cryptoFirst: '',
      cryptoSecond: '',
      error: '',
      result: 0,
      currentYear: ''
    }
  },
  created() {
    this.setCurrentYear()
  },
  methods: {
    changeAmount(val) {
      this.amount = val
      if (!val) {
        this.clearState()
      } else {
        this.convert()
      }
    },
    setCryptoFirst(val) {
      this.cryptoFirst = val
      this.convert()
    },
    setCryptoSecond(val) {
      this.cryptoSecond = val
      this.convert()
    },
    clearState() {
      this.result = ''
      this.error = ''
      this.cryptoFirst = ''
      this.cryptoSecond = ''
      this.$refs.cryptoList.clearActive()
    },

    async convert() {
      if (!this.cryptoFirst || !this.cryptoSecond) {
        this.error = ''
        return
      }

      if (this.amount <= 0) {
        this.error = 'Введите число больше 0'
        return
      } else if (this.cryptoFirst === this.cryptoSecond) {
        this.error = 'Выберите другую валюту'
        return
      }
      this.error = ''

      await convert.ready()

      const conversionMap = {
        USDT_BTC: convert.USDT.BTC,
        USDT_ETH: convert.USDT.ETH,
        USDT_LTC: convert.USDT.LTC,
        USDT_LINK: convert.USDT.LINK,
        USDT_XRP: convert.USDT.XRP,
        USDT_SOL: convert.USDT.SOL,
        USDT_TON: convert.USDT.TON,

        BTC_USDT: convert.BTC.USDT,
        BTC_ETH: convert.BTC.ETH,
        BTC_LTC: convert.BTC.LTC,
        BTC_LINK: convert.BTC.LINK,
        BTC_XRP: convert.BTC.XRP,
        BTC_SOL: convert.BTC.SOL,
        BTC_TON: convert.BTC.TON,

        ETH_USDT: convert.ETH.USDT,
        ETH_BTC: convert.ETH.BTC,
        ETH_LTC: convert.ETH.LTC,
        ETH_LINK: convert.ETH.LINK,
        ETH_XRP: convert.ETH.XRP,
        ETH_SOL: convert.ETH.SOL,
        ETH_TON: convert.ETH.TON,

        LTC_USDT: convert.LTC.USDT,
        LTC_BTC: convert.LTC.BTC,
        LTC_ETH: convert.LTC.ETH,
        LTC_LINK: convert.LTC.LINK,
        LTC_XRP: convert.LTC.XRP,
        LTC_SOL: convert.LTC.SOL,
        LTC_TON: convert.LTC.TON,

        LINK_USDT: convert.LINK.USDT,
        LINK_BTC: convert.LINK.BTC,
        LINK_ETH: convert.LINK.ETH,
        LINK_LTC: convert.LINK.LTC,
        LINK_XRP: convert.LINK.XRP,
        LINK_SOL: convert.LINK.SOL,
        LINK_TON: convert.LINK.TON,

        XRP_USDT: convert.XRP.USDT,
        XRP_BTC: convert.XRP.BTC,
        XRP_ETH: convert.XRP.ETH,
        XRP_LTC: convert.XRP.LTC,
        XRP_LINK: convert.XRP.LINK,
        XRP_SOL: convert.XRP.SOL,
        XRP_TON: convert.XRP.TON,

        SOL_USDT: convert.SOL.USDT,
        SOL_BTC: convert.SOL.BTC,
        SOL_ETH: convert.SOL.ETH,
        SOL_LTC: convert.SOL.LTC,
        SOL_LINK: convert.SOL.LINK,
        SOL_XRP: convert.SOL.XRP,
        SOL_TON: convert.SOL.TON,

        TON_USDT: convert.TON.USDT,
        TON_BTC: convert.TON.BTC,
        TON_ETH: convert.TON.ETH,
        TON_LTC: convert.TON.LTC,
        TON_LINK: convert.TON.LINK,
        TON_XRP: convert.TON.XRP,
        TON_SOL: convert.TON.SOL
      }

      const key = `${this.cryptoFirst}_${this.cryptoSecond}`
      const conversionFunction = conversionMap[key]

      if (conversionFunction) {
        this.result = `${conversionFunction(this.amount)} ${this.cryptoSecond}`
      } else {
        this.error = 'Такой пары нет'
      }
    },
    setCurrentYear() {
      const year = new Date().getFullYear()
      this.currentYear = year
    }
  }
}
</script>

<template>
  <main className="wrapper">
    <h1 className="title">Crypto currency</h1>
    <InputSection :changeAmount="changeAmount" :convert="convert" />
    <p v-if="error !== ''" className="error">{{ error }}</p>
    <p v-if="result !== 0" className="result">{{ result }}</p>
    <div className="cryptoLists">
      <CryptoList :setCrypto="setCryptoFirst" />
      <CryptoList :setCrypto="setCryptoSecond" />
    </div>
  </main>
  <footer className="footer">
    <a href="https://nice-dev.ru/" className="nice-dev" target="_blank">
      © Nice Dev Web-Studio 2020 - {{ currentYear }}
    </a>
  </footer>
</template>

<style scoped>
.wrapper {
  flex: 1;
  position: relative;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 100px 10px 10px;
}
.title {
  font-size: 56px;
  color: #e7e7e7;
  text-shadow: 3px 5px 5px rgba(0, 0, 0, 1);
  letter-spacing: 3px;
}
input {
  width: 100%;
  border-radius: 3px;
  font-size: 16px;
  border: none;
  padding: 10px;
  background-color: rgba(255, 255, 255, 0.7);
  color: #333;
  box-shadow: inset 0 -3px 10px -5px rgba(0, 0, 0, 1);
  outline: none;
}
.error {
  position: absolute;
  font-size: 14px;
  color: #f3bbbb;
  margin: 10px 0 0;
}
.result {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  color: #fff;
  margin: 50px 0 0;
  font-size: 30px;
}
.cryptoLists {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
  margin: 150px auto 0;
}
.nice-dev {
  display: block;
  margin: 100px auto 10px;
  width: fit-content;
  text-decoration: none;
  color: #fff;
  font-size: 12px;
  transition: 0.3s;
}

.nice-dev:hover {
  text-shadow: 0 0 10px #fff;
  color: #fff;
}
</style>
