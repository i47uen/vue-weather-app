<template>
  <div id="app">
    <main>

      <span class="bookmarks" @click="bm_isActive = !bm_isActive" :class="{active: bm_isActive}">{{ bm_isActive ? "-" : "+" }}</span>
      <div class="bookmarks-list" :class="{active: bm_isActive}">
        <ul>
          <li v-for="(city, index) in bookmarks" :key="city.title" @click="setCurrentCity(city.title)">
            <p>{{ city.title }}</p> <span @click="bookmarks.splice(index, 1)" class="remove" >x</span>
          </li>
        </ul>
      </div>

      <div class="search-box">
        <input
            type="text" class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress.enter="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}
            <span @click="addToBookmarks()">★</span>
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°с</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>

      <div class="empty-form" v-else>
        <img width="250" src="./assets/gps.svg" alt="gps.svg">
        <p>Enter the city in the field above</p>
      </div>

    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    api_key: "aacd333c6dbae34ec34ccffc98a54abe",
    url_base: "https://api.openweathermap.org/data/2.5/",
    query: "",
    weather: {},
    bm_isActive: false,
    bookmarks: [
      {title:"Moscow"}
    ]
  }),
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
    setCurrentCity(title){
      this.query = title
      this.fetchWeather()
    },
    addToBookmarks(){
      this.bookmarks.push({title: this.weather.name })
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

.empty-form{
  img{
    margin-bottom: 30px;
    margin-top: 10px;
  }
  color: #cccccc;
  font-weight: 600;
  font-size: 2.5em;
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

.bookmarks{
  cursor: pointer;
  font-size: 65px;
  line-height: 0;
  color: #cccccc;
  user-select: none;
  text-shadow: 0 0 4px rgba(black, 0.25);
  display: inline;
  position: absolute;
  top: 88px;
  right: 65px;
  font-family: monospace;
  font-weight: 900;
  letter-spacing: -8px;
  z-index: 10;
  &.active{
    color: #2c3e50;
  }
  &-list{
    transform: translateX(100%);
    transition: all 0.2s ease-in-out;

    &.active{
      transform: translateX(0);
    }

    position: absolute;
    right: 0;
    color: #2c3e50;
    background: #cccccc;
    padding: 25px;
    height: 100vh;
    width: 30%;
    ul{
      margin-top: 110px;
      max-height: 75vh;
      padding: 10px 5px;
      overflow: auto;
      li{
        border: 1px solid #ccc;
        padding: 5px 20px;
        padding-top: 9px;
        margin: 15px 0;
        text-align: left;
        list-style: none;
        user-select: none;

        display: flex;
        justify-content: space-between;
        align-items: center;

        cursor: pointer;
        p{
          font-weight: 600;
        }
        box-shadow: 0 0 10px rgba(black, 0.15);
        transition: all 0.1s linear;
        &:hover{
          background: rgba(#fff, 0.25);
          box-shadow: none;
        }
        .remove{
          display: block;
          font-family: sans-serif;
          z-index: 10;

          font-size: 22px;
          font-weight: 500;
          cursor: pointer;
          z-index: 10;
          padding: 10px;
          transition: all 0.1s linear;
          &:hover{
            transform: scale(1.3);
            color: orangered;
          }
        }
      }
    }
  }
}
</style>
