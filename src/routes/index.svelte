<script>
  import { onMount } from "svelte";
  import axios from "axios";

  let data;
  let confirmed;
  let recovered;
  let deaths;
  let lastUpdate;

  onMount(() => {
    // getGlobalApi();
    get();
  });

  function get() {
    axios.get("https://covid19.mathdro.id/api").then(function(response) {
      data = response.data;
      confirmed = data.confirmed.value;
      recovered = data.recovered.value;
      deaths = data.deaths.value;
      lastUpdate =
        new Date(data.lastUpdate).toDateString() +
        " - " +
        new Date(data.lastUpdate).toTimeString();
      console.log(data);
    });
  }

  async function getGlobalApi() {
    const res = await fetch("https://covid19.mathdro.id/api").then(response =>
      response.json()
    );
    data = res;
    // console.log(data);
  }
</script>

<style>
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

<h1>Covid19 Global Summary</h1>
<div class="summary-container">
  <div class="summary-card confirmed-container">
    <h1>Infected</h1>
    <h2>{confirmed === undefined ? '.....' : confirmed}</h2>
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
