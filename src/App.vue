<template>
 <div class="container">
    <div class="row">
      <input
        type="text"
        class="form-control text-light bg-dark rounded-2 my-4 "
        placeholder="Search"
        v-model="textSearch"
        @keyup="searchCoin()"
        autofocus
      />

      <table class="table table-hover table-dark text-light">
        <thead>
          <tr>
            <th v-for="(title, index) in titles" :key="index">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin) in filteredCoins" :key="coin.id">
            <td>
              <img :src="coin.image" :alt="coin.name" style="width: 2rem" class="me-2" />
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-2 text-muted text-uppercase">
                {{ coin.symbol }}
              </span>
            </td>
            <td>${{ coin.current_price.toLocaleString() }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }}
            </td>
            <td>${{coin.market_cap.toLocaleString()}}</td>
            <td>{{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  components: {},
  data () {
    return {
      coins: [],
      filteredCoins: [],
      titles: ['Nome', 'Preço', 'Variação 24h', 'Capitalização do Mercado', 'Volume 24h'],
      textSearch: ''
    }
  },
  async mounted () {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&price_change_percentage=1h')
    const data = await res.json()
    this.coins = data
    this.filteredCoins = data
  },
  methods: {
    searchCoin () {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      )
    }
  }
}
</script>

<style>
</style>
