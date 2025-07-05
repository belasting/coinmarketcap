<template>
  <div class="container">
    <h1>Coin Market</h1>
    <a href="https://github.com/belasting/coinmarketcap">
      <button class="knopje">Sourcecode</button>
    </a>
    <search-coins @search="logChange" />

    <table class="table table-dark">
      <thead>
        <tr>
          <th v-for="title in titles" :key="title">
            {{ title }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(coin, index) in filterCoins" :key="coin.id">
          <td>
            {{ index + 1 }}
          </td>
          <td>
            <img :src="coin.image" style="width: 2rem" class="me-2" />
            <span>{{ coin.name }}</span>
            <span class="ms-2 text-uppercase text-muted">
              {{ coin.symbol }}
            </span>
          </td>
          <td>
            {{ coin.current_price }}
          </td>
          <td
            :class="[
              coin.price_change_percentage_24h > 0
                ? 'text-success'
                : 'text-danger',
            ]"
          >
            {{ coin.price_change_percentage_24h }} %
          </td>
          <td>$ {{ coin.total_volume.toLocaleString() }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import SearchCoins from "./components/SearchCoin.vue";

export default {
  components: {
    SearchCoins,
  },
  name: "App",
  data() {
    return {
      coins: [],
      filterCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h Volumen"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    console.log(data);
    this.coins = data;
    this.filterCoins = data;
  },
  methods: {
    searchCoin() {
      this.filterCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
    logChange(text) {
      this.textSearch = text;
      console.log(text);
      this.searchCoin();
    },
  },
};
</script>

<style>
</style>
