<template>
  <div id="app">
    <main>
      <div class="searh-box">
        <input type="text" class="search-bar" placeholder="Search..." />
      </div>
      <div class="weather-data">
        <p class="location">London, GB</p>
        <p class="date">Sunday, 12 January, 2020</p>
        <p class="temp">7°C</p>
        <p class="weather">Clouds</p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "d1c59648c144681a6a487b45f5b0c58b",
    };
  },
};
</script>

<style>
</style>
