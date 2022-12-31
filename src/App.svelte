<script>
import { onMount } from "svelte";


  let titles = ['#', 'Coin', 'Price', '24 changes', 'Total volume']
  let coins = []
  let filteredCoins = []
  let ref = undefined

const loadCoins = async () => {
 
 const res = await fetch ('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')

 const data = await res.json()

 coins = data
 filteredCoins = data
}

loadCoins();

const searchCoin = (e) => {
  filteredCoins = coins.filter(coin => coin.name.toLowerCase().includes(e.target.value.toLowerCase()) || coin.symbol.toLowerCase().includes(e.target.value.toLowerCase()));
}

onMount(() => ref.focus())

</script>

<div class="container">
  <div class="row">

    <div class="form-group">
       <label class="col-form-label col-form-label-lg mt-4" for="inputLarge">Search:</label>
  <input class="form-control form-control-lg" type="text" placeholder="Search your coin" id="inputLarge" on:keyup={searchCoin} bind:this={ref}>
    </div>

<table class="table table-hover">
  <thead>
    <tr>
      {#each titles as title}
      <th scope="col">{title}</th>
      {/each}
    </tr>
  </thead>
  <tbody>
    {#each filteredCoins as coin}
      <tr class="table-active">
        <td class="text-muted">
          {coin.market_cap_rank}
        </td>
        <td>
          <img class="image img-fluid me-2" src={coin.image} alt={coin.name}>
          <span>
            {coin.name}
          </span>
            <small>
              <span class="badge bg-secondary text-uppercase">
              {coin.symbol}
              </span>
            </small>
          </td>
          <td>
            ${coin.current_price.toLocaleString()}
          </td>
          <td class={coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'}>
            {coin.price_change_percentage_24h} %
          </td>
          <td>
            ${coin.total_volume.toLocaleString()}
          </td>
      </tr>
    {/each}
  </tbody>
</table>
</div>
</div>

<style>
  .image {
    width: 1.2rem;
  }
</style>
