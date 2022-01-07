<template>
  <main>
    <form class="search-box" @submit.prevent="fetchWeather()">
      <input
        type="text"
        class="search-bar"
        placeholder="search..."
        v-model="query"
      />
    </form>

    <section class="weather-wrap" v-if="weather.main != undefined">
      <div class="location-box">
        <h1 class="location">{{ weather.name }}, {{weather.sys.country}}</h1>
        <p class="date">{{actualDate}}</p>
      </div>

      <article class="weather-box">
        <h1 class="temp">{{convertFtoC}}°C</h1>
        <p class="weather">{{weather.weather[0].description}}</p>
        <p>ahoj</p>
      </article>
    </section>
  </main>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      api_key: "7a082053525dbc86241caf7595cf1eae",
      base_url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      date:" ",
    };
  },
  methods: {
    async fetchWeather() {
      let response = await fetch(
        `${this.base_url}weather?q=${this.query}&appid=${this.api_key}`
      );
      let data = await response.json();
      this.weather = await data; //response napcham do weather objectu v datach
      // this.query = "";
      console.log(this.weather);
    },

    // fetchWeather() {
    //   fetch(`${this.base_url}weather?q=${this.query}&appid=${this.api_key}`)
    //     .then((res) => res.json())
    //     .then((data) => {
    //       this.weather = data;
    //       console.log(this.weather);
    //     });
    //   this.query = "";
    // },

    
  },
  computed: {
    convertFtoC() {
      return Math.floor((this.weather.main.temp -32)*5 / 9) 
    },
    //today
    actualDate(){
      let today = new Date();
      // return today.getFullYear()+'-'+(today.getMonth()+1)+'-'+today.getDate();
      let days =['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday']
      return days[today.getDay()]+'   '+today.getDate()+'.'+(today.getMonth()+1)+'.'+today.getFullYear();
    }
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
  background-image: url("./assets/day.jpg");
  background-size: cover;
  background-position: bottom;

  transition: 0.4s;
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
  max-width: 300px;
  margin-bottom: 30px;
  margin: 0 auto 30px auto;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 1.2rem;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.search-bar .search-bar:focus {
  background: rgba(255, 255, 255, 0.75);
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  border-radius: 16px 0 16px 0;
}
.location-box .location {
  color: #fff;
  font-size: 2rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 1.2rem;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 6.375rem;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 1rem;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 3rem;
  font-weight: 700;
  font-style: italic;
}
</style>
