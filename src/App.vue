<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div v-if="typeof weather.main != 'undefined'" class="weather-wrap">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "57575ff764ae0ce3f6bc5f2f4b4cafd1",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
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
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
      console.log(this.weather);
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

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@500;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
}
#app {
  background: url("./assets/cold-bg.jpeg");
  background-size: cover;
  background-position: bottom;
  transition: 0.3s;

  &.warm {
    background: url("./assets/warm-bg.jpeg");
    background-size: cover;
    background-position: bottom;
    transition: 0.3s;
  }
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

.search-box {
  width: 100%;
  margin-bottom: 30px;

  .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 20px;

    color: #313131;

    appearance: none;
    outline: none;
    background: none;
    border: none;

    border-radius: 10px;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);

    background-color: #fff;
    opacity: 0.5;

    transition: 0.4s;

    &:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      border-radius: 0px 10px 0px 10px;
      opacity: 0.8;
    }
  }
}

.location-box {
  .location {
    color: #fff;
    font-weight: 500;
    font-size: 32px;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .date {
    color: #fff;
    font-weight: 300;
    font-size: 20px;
    font-style: italic;
    text-align: center;
  }
}

.weather-box {
  text-align: center;

  .temp {
    display: inline-block;
    padding: 10px 25px;
    color: #fff;
    font-size: 102px;
    font-weight: 900;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba($color: #fff, $alpha: 0.25);
    border-radius: 16px;
    margin: 30px 0px;

    box-shadow: 3px 6px rgba($color: #000000, $alpha: 0.25);
  }

  .weather {
    color: #fff;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba($color: #000000, $alpha: 0.25);
  }
}
</style>
