<script>
export default {
  props: {
    setCrypto: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      current: '',
      currencies: ['USDT', 'BTC', 'ETH', 'LTC', 'LINK', 'XRP', 'SOL', 'TON']
    }
  },
  methods: {
    selectItem(val) {
      this.setCrypto(val)
      this.current = val
    }
  }
}
</script>

<template>
  <ul>
    <li
      v-for="currency in currencies"
      :key="currency"
      @click="selectItem(currency)"
      :class="{ active: current === currency }"
    >
      {{ currency }}
    </li>
  </ul>
</template>

<style scoped>
ul {
  padding: 0;
  border: 1px solid #fff;
  color: #fff;
  font-size: 24px;
  border-radius: 3px;
}
li {
  line-height: 1.5;
  cursor: pointer;
  transition: 0.3s;
}
li:not(:last-of-type) {
  border-bottom: 1px solid #fff;
}
li:hover,
li.active {
  background-color: #239bd5;
}
</style>
