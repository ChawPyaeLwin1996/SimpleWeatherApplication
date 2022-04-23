<template>
  <div>
    <main>
      <!-- Search City Bar -->
      <div class="search-box">
        <div style="font-style:oblique:30px;color:rgb(236 210 16);">
          Search by city name!
        </div>
        <input
          type="text"
          class="search-bar"
          placeholder="Yangon"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <!--Weather Result -->
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            Today's forecast . {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">
            {{ Math.round(weather.main.temp) }}°c
            <div class="weather">
              <i class="fa fa-cloud" style="font-size: 30px; color: #dcc61e"></i
              >{{ weather.weather[0].main }}
            </div>
          </div>
        </div>
      </div>

      <div class="author">
        <span>© Chaw Pyae Lwin 2022</span>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "cf8e328a0956d95fbc6cd69269618ceb",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
    };
  },
  /*Get initial data */
  async mounted() {
    this.weather = await this.getResponse();
  },

  methods: {
    /*Get response data to show Yangon as default for initial state*/
    getResponse() {
      return axios
        .get(
          "http://api.openweathermap.org/data/2.5/weather?q=Yangon&units=metric&APPID=cf8e328a0956d95fbc6cd69269618ceb"
        )
        .then((response) => response.data);
    },

    /*Fetch weather using OpenWeatherMap API by event*/
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((data) => {
            return data.json();
          })
          .then(this.setResults);
      }
    },

    /*To Display weather result*/
    setResults(results) {
      this.weather = results;
    },

    /*Get Date*/
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
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;300;500;700;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Poppins", sans-serif;
  font-size: 16pt;
}

#app {
  background-image: url("./assets/weather.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.2s;
  max-width: 100%;
}

main {
  min-height: 100vh;
  padding: 60px 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );

  .search-box {
    width: 100%;
    margin-bottom: 30px;

    @media screen and (min-width: 768px) {
      width: 50%;
      margin: 0 auto 30px;
    }

    .search-bar {
      display: block;
      width: 100%;
      padding: 15px;
      color: #000000;
      font-size: 1.1rem;
      appearance: none;
      border: none;
      outline: none;
      background: none;
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 0px 16px 0px 16px;
      transition: 0.4s;
    }

    .search-bar:focus {
      box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.75);
      border-radius: 16px 0px 16px 0px;
    }
  }

  .logo-container {
    max-width: 130px;
    padding: 0px 0px 20px;

    @media screen and (min-width: 768px) {
      max-width: 230px;
      margin: 0 auto;
      padding: 0px 20px 20px;
    }

    img {
      width: 100%;
      height: auto;
    }
  }

  .location-box {
    .location {
      color: skyblue;
      font-size: 2rem;
      font-weight: 500;
      text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
      margin-left: 3rem;
      margin-top: 3rem;
    }

    .date {
      color: skyblue;
      font-size: 1.5rem;
      font-weight: 300;
      font-style: italic;
      margin-left: 4rem;
    }
  }

  .author {
    position: absolute;
    bottom: 0;
    width: 100%;
    text-align: center;
    padding: 10px;
    font-weight: 300;
  }

  .author span {
    display: block;
    color: #fff;
    font-size: 0.75rem;
  }

  .author a {
    color: yellow;
  }
  .weather-wrap {
  }
  .weather-box {
    text-align: center;
    display: block;
    width: 30%;
    margin-left: 3rem;
    @media screen and (max-width: 768px) {
      width: 70%;
    }

    .temp {
      color: #fff;
      font-size: 4.5rem;
      font-weight: 700;
      text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
      background-color: rgba(255, 255, 255, 0.25);
      border-radius: 16px;
      margin: 30px 0px;
      box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
      height: 300px;
      padding-top: 4rem;
    }

    .weather {
      color: #fff;
      font-size: 1.5rem;
      font-weight: 500;
      font-style: italic;
      text-alifn: center;
    }

    .search-text {
      color: #aaefdf;
    }
  }
}
</style>
