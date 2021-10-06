<template>
  <div
    id="app"
    v-bind:class="weather.weather ? weather.weather[0].main : 'Clouds'"
  >
    <main>
      <input
        type="text"
        class="search-bar"
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
      />
      <div class="weather-data" v-if="weather.main !== undefined">
        <img
          v-bind:src="
            'http://openweathermap.org/img/wn/' +
            weather.weather[0].icon +
            '@2x.png'
          "
          v-bind:alt="weather.weather[0].main"
        />
        <p class="location">{{ weather.name }}, {{ weather.sys.country }}</p>
        <p class="date">{{ dateBuilder() }}</p>
        <p class="temp">{{ Math.round(weather.main.temp) }}°C</p>
        <p class="min-max">
          <span class="temp-low">Low:</span>
          {{ Math.round(weather.main.temp_min) }}°C
          <span class="temp-high">High:</span>
          {{ Math.round(weather.main.temp_max) }}°C
        </p>
        <p class="weather">{{ weather.weather[0].description }}</p>
      </div>
      <div class="weather-data" v-if="weather.main === undefined">
        <p class="date">Please select another location</p>
      </div>

      <table class="forecast" v-if="weather.main !== undefined">
        <tr v-for="line in weatherForecast" :key="line.date">
          <th>{{ line.date }}</th>
          <td v-for="item in line.items" :key="item.dt_txt">
            <p>{{ item.dt_txt }}</p>
            <img
              v-bind:src="
                'http://openweathermap.org/img/wn/' +
                item.weather[0].icon +
                '@2x.png'
              "
              v-bind:alt="weather.weather[0].main"
            />
            <p>{{ Math.round(item.main.temp) }}°C</p>
          </td>
        </tr>
      </table>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "d1c59648c144681a6a487b45f5b0c58b",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      weatherForecast: {},
    };
  },
  created() {
    fetch(
      `${this.url_base}weather?q=seattle&units=metric&APPID=${this.api_key}`
    )
      .then((res) => {
        return res.json();
      })
      .then(this.setResult);
    // fetch(
    //   `${this.url_base}forecast?q=seattle&units=metric&APPID=${this.api_key}`
    // )
    //   .then((res) => {
    //     return res.json();
    //   })
    //   .then(this.setForecastResult);
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResult);
      }
    },
    setResult(result) {
      this.weather = result;

      if (this.query == "") {
        this.query = "Seattle";
      }
      if (this.weather.main !== undefined) {
        fetch(
          `${this.url_base}forecast?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setForecastResult);
      }
    },
    setForecastResult(result) {
      let newList = [{}];
      let index = 0;
      let months = [
        "Jan",
        "Feb",
        "Mar",
        "Apr",
        "May",
        "Jun",
        "Jul",
        "Aug",
        "Sep",
        "Oct",
        "Nov",
        "Dec",
      ];
      for (let i = 0; i < result.list.length; i++) {
        let dateTime = result.list[i].dt_txt.split(" ");
        let convertDate = dateTime[0].slice(5, 11).split("-");
        let date = months[convertDate[0] - 1] + " " + convertDate[1];
        let time = dateTime[1];
        if (newList[index].date !== date) {
          index++;
          result.list[i].dt_txt = time.slice(0, 5);
          let item = {
            date: date,
            items: [result.list[i]],
          };
          newList.push(item);
        } else {
          result.list[i].dt_txt = time.slice(0, 5);
          newList[index].items.push(result.list[i]);
        }
      }
      newList.shift();
      this.weatherForecast = newList;
    },
    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app .Clouds {
  background-image: url("./assets/clouds.jpg");
}
#app .Clear {
  background-image: url("./assets/clear.jpg");
}
#app .Snow {
  background-image: url("./assets/snow.jpg");
}
#app .Rain {
  background-image: url("./assets/rain.jpg");
}
#app .Thunderstorm {
  background-image: url("./assets/thunderstorm.jpg");
}
#app .Mist {
  background-image: url("./assets/mist.jpg");
}
#app .Smoke {
  background-image: url("./assets/smoke.jpg");
}
#app .Haze {
  background-image: url("./assets/haze.jpg");
}
#app .Fog {
  background-image: url("./assets/fog.jpg");
}
#app .Sand {
  background-image: url("./assets/sand.jpg");
}
#app .Dust {
  background-image: url("./assets/dust.jpg");
}
#app .Ash {
  background-image: url("./assets/ash.jpg");
}
#app .Squall {
  background-image: url("./assets/squall.jpg");
}
#app .Tornado {
  background-image: url("./assets/tornado.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  /* appearance: none; */
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;

  transition: 0.4s;
  margin-bottom: 30px;
}
.search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.weather-data {
  color: #fff;
  text-align: center;
}
.weather-data .location {
  font-size: 32px;
  font-weight: 500px;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-data .date,
.weather-data .min-max {
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
}
.weather-data .min-max .temp-low {
  color: rgb(238, 183, 116);
}
.weather-data .min-max .temp-high {
  margin-left: 20px;
  color: indianred;
}
.weather-data .temp {
  display: inline-block;
  padding: 10px 25px;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-data .weather {
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  text-transform: capitalize;
}
table {
  width: 100%;

  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  color: #fff;
  overflow: hidden;

  table-layout: fixed;
}
table th {
  white-space: nowrap;
}
table tr {
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 10px;

  overflow-x: scroll;

  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}
table tr::-webkit-scrollbar {
  display: none;
}
table tr:nth-child(odd) {
  background-color: rgba(255, 255, 255, 0.5);
  color: #313131;
}
table tr td {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
  padding: 0 10px;
}
table tr td img {
  width: 45px;
}
</style>
