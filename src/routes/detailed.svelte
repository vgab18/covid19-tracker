<script>
  import { onMount } from "svelte";
  import Chart from "chart.js";

  let countries = [];
  let country = "Afghanistan";
  let infected;
  let recovered;
  let deaths;
  let lastUpdate;

  onMount(() => {
    getCountries();
    getCountryData();
  });

  async function getCountryData() {
    const res = await fetch(
      "https://covid19.mathdro.id/api/countries/" + country
    );
    const json = await res.json();
    infected = json.confirmed.value;
    recovered = json.recovered.value;
    deaths = json.recovered.value;
    lastUpdate = new Date(json.lastUpdate).toDateString();
  }

  async function getCountries() {
    const res = await fetch("https://covid19.mathdro.id/api/countries");
    const json = await res.json();
    countries = json.countries;
    country = countries[0].name;
  }

  function renderChart() {}

  function handleChangeCountry(e) {
    country = e.target.value;
    getCountryData();
  }
</script>

<style>
  select {
    margin: 0 auto;
    width: 60%;
    padding: 10px;
    font-size: 15px;
  }
  .summary-container {
    display: flex;
  }
  .summary-card {
    margin: 5rem auto;
    padding: 30px;
    border-radius: 5px;
    color: white;
    width: 20%;
  }
  .update-container {
    margin: 3.5rem auto;
    text-align: center;
  }
  .confirmed-container {
    background-color: #e84393;
  }
  .recovered-container {
    background-color: #00b894;
  }
  .deaths-container {
    background-color: #d63031;
  }
</style>

<svelte:head>
  <title>Covid19 Tracker</title>
</svelte:head>
<h1>Country statistics</h1>
<center>
  <select value={country} on:change={handleChangeCountry}>
    {#if countries === undefined || countries === null}
      <option>.....</option>
    {:else}
      {#each countries as country}
        <option value={country.name}>{country.name}</option>
      {/each}
    {/if}
  </select>
</center>
<div class="summary-container">
  <div class="summary-card confirmed-container">
    <h1>Infected</h1>
    <h2>{infected === undefined ? '.....' : infected}</h2>
  </div>
  <div class="summary-card recovered-container">
    <h1>Recovered</h1>
    <h2>{recovered === undefined ? '.....' : recovered}</h2>
  </div>
  <div class="summary-card deaths-container">
    <h1>Deaths</h1>
    <h2>{deaths === undefined ? '.....' : deaths}</h2>
  </div>
</div>
<div class="update-container">
  <h4>Last Update: {lastUpdate}</h4>
</div>
