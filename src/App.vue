<template>
  <!-- Entry point of the VueJs application -->
  <!-- Checks that the main content of the object retrieved in the request exists & changes the background image depending on the temperature -->
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 18 ? 'warm' : ''">
    <main>
      <h3>Quel temps fait-il dans votre ville préférée ?</h3>
      <section class="search-box">
        <!-- v-model creates a link between a form entry element or a component -->
        <!-- @keypress calls the method concerned at the press of a key, here enter -->
        <input
          type="text"
          class="search-bar"
          placeholder="Recherche..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </section>

      <!-- Checks that the main content of the object retrieved in the request exists -->
      <section class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{ capitalizeFirstLetter(moment()) }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          <div>
            <img :src="'https://openweathermap.org/img/wn/'+ weather.weather[0].icon + '@2x.png'" />
          </div>
          <!-- Retrieves the last weather event -->
          <div class="weather">{{capitalizeFirstLetter(weather.weather[0].description)}}</div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import moment from "moment";
import localization from "moment/src/locale/fr";

export default {
  name: "App",
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      url_base: process.env.VUE_APP_URL_BASE,
      query: "", // API request
      weather: {} // to store data
    };
  },
  methods: {
    // Fetch recovers resources through the API asynchronously
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}&lang=fr`
        )
          .then(response => {
            return response.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    moment() {
      return moment()
        .locale("fr", localization)
        .format("dddd Do MMMM YYYY");
    },
    capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Patrick Hand", cursive;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
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

h3 {
  margin: 15px 0;
  color: #fff;
  text-align: center;
  font-size: 1.2rem;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 30px;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin-bottom: 6px;
}

.location-box .date {
  color: #fff;
  font-size: 18px;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

img {
  filter: drop-shadow(3px 6px rgba(0, 0, 0, 0.5));
}

.weather-box .weather {
  color: #fff;
  font-size: 46px;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>

