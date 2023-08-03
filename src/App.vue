<script>

  export default {
    name: "App", 
    data(){
      return {
        coins: [],
        coins2: [],
        titles: [
          '#',
          'Moneda',
          'Precio',
          'Precio Cambio',
          '24h Volumen'
        ],
        textSearch: "",
      };
    },   
    async mounted(){
      const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
      const data1 = await res.json()
      //console.log(data1);
      this.coins = data1
      this.coins2 = data1
    },
    methods: {
      searchCoin() {
        //SOLO ESTRIG EXACTO  this.coins.filter(coin => coin.name === this.textSearch);
        this.coins2 = this.coins.filter(coin => coin.name.toLowerCase().includes(this.textSearch.toLocaleLowerCase()) ||
        coin.symbol.toLocaleLowerCase().includes(this.textSearch.toLocaleLowerCase()));
      },
      redirectToImagePage() {
      // Redireccionar a otra página con imagen incrustada.
      window.location.href = 'src/imagen.html';
      }
    }
  }
</script>

<template>
  <div class="container">
  <div class="row">  
  <h1>Martin Dorantes Martínez, Cripto Monedas</h1>
  <button @click="redirectToImagePage">Cerrar Página Actual y Mostrar página con imagen </button>
  <a href="src/imagen.html" target="_blank"><button > NO Cerrar Página Actual e Ir a Página Nueva con imagen </button></a>
  <input type="text" class="form-control bg-dark text-light rounded-0 my-4" 
    placeholder="Buscar Moneda"
    @keyup="searchCoin()"
    v-model="textSearch"
  />

  <table class="table table-hover table-dark text-light">
    <thead>
      <tr>
        <th v-for="title in titles" :key="title">
          {{ title }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(coin, index) in coins2" :key="coin.id">
        <td>
          {{ index + 1}}
        </td>
        <td>
          <img :src="coin.image" style="width: 2rem" class="me-2" />
          <span>
            {{ coin.name }}
          </span>
          <span class="ms-2 text-uppercase text-muted">
            {{ coin.symbol }}
          </span>
        </td>
        <td>
          $ {{ coin.current_price }}
        </td>
        <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">
          {{ coin.price_change_percentage_24h }}%
        </td>
        <td>
          $ {{ coin.total_volume.toLocaleString() }}
        </td>

      </tr>
    </tbody>
  </table>
  </div>   
  </div>
</template>

<style scoped>
</style>
