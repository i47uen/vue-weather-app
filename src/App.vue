<template>
  <div id="app">
    <main>

<!-- <Bookmarks />-->

      <div class="search-box">
        <input
            type="text" class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}
            <span>★</span>
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°с</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>

      <h1 class="err-empty" v-else>You didn't enter a city name <br> or the weather forecast for this city is not available.</h1>

    </main>
  </div>
</template>

<script>
// import Bookmarks from "./components/Bookmarks"
export default {
  name: 'App',
  data: () => ({
    api_key: "aacd333c6dbae34ec34ccffc98a54abe",
    url_base: "https://api.openweathermap.org/data/2.5/",
    query: "",
    weather: {},
    bookmarks: []
  }),
  components: {
    // Bookmarks
  },
  methods: {
    fetchWeather(){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults)
    },
    setResults(results){
      this.weather = results;
    },
    setWeather(newQuery){
      this.query = newQuery;
      this.fetchWeather()
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July",
        "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
      "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style lang="scss">
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  overflow: hidden;
}
#app {
  background-image: url("assets/bg.jpg");
  background-size: cover;
  background-position: bottom;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
main{
  min-height: 100vh;
  background-image: linear-gradient(to bottom, rgba(black, 0.25), rgba(black, 0.75));
}

.err-empty{
  color: #ffffff;
  font-weight: 400;
  line-height: 50px;
}
.search-box{
  width: 100%;
  padding-top: 5vh;
  padding-bottom: 10vh;
  margin-bottom: 30px;
}
.search-bar{
  transition: all 0.2s linear;
  display: block;
  width: 60%;
  margin: 0 auto;
  padding: 15px 25px;
  color: #2c3e50;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background: rgba(white, 0.5);
  border-radius: 0 16px 0 16px;
  box-shadow: 0 0 20px rgba(black, 0.25);
  &:focus{
    border-radius: 0;
    background: rgba(white, 0.7);
    //outline: 2px solid #2c3e50;
  }
}
.weather-wrap{
  color: #ffffff;
  text-shadow: 1px 3px rgba(black, 0.25);
  .location{
    font-size: 38px;
    font-weight: 600;
    position: relative;
    span{
      position: absolute;
      top: -10px;
      margin-left: 15px;
      color: rgba(white, 0.25);
      text-shadow: none;
      cursor: pointer;
      user-select: none;
      &:hover{
        color: rgba(white, 0.65);
      }
    }
  }
  .date{
    font-style: italic;
    font-weight: 300;
    color: #ccc;
  }
  .weather-box{
    .temp{
      display: inline-block;
      padding: 10px 25px;
      padding-top: 28px;
      margin: 30px 0;
      font-size: 80px;
      border-radius: 16px;
      font-weight: 600;
      background-color: rgba(white, 0.25);
      box-shadow: 0 0 20px rgba(black, 0.25);
    }
    .weather{
      font-size: 48px;
      font-weight: 700;
    }
  }
}
</style>
