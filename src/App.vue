<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
    <main>
      <div v-if="loading" class="loader">
          <!-- here put a spinner or whatever you want to indicate that a request is in progress -->
      </div>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        >
      </div>
      <div class="weather-wrap" :class="typeof weather.main != 'undefined' ? 'hide' : 'show'">
        <div class="location-box">
          <div class="location">
            {{weather["message"]}}
          </div>
        </div>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }},  {{ weather.sys.country }}
          </div>
          <div class="date">
           {{ dateBuilder()}}
          </div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class='weather'>{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>
 
<script>

export default {
  name: 'App',
  data (){
    return {
      api_key: 'eea5128d0c0092add2e4244dc7075f26',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      loading: false,
      showEmpty: false,
    }
  },
  methods: {
    fetchWeather (e) {
      if(e.key == "Enter") {
        this.loading = true //the loading begin
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
      this.loading = false;
    },
    dateBuilder () {
      let d = new Date();
      let months = ['January','February','March','April','May',
                    'June','July','August','September','October',
                    'November','December'];
      let days = ['Sunday','Monday','Tuesday','Wednesday',
                  'Thursday','Friday','Saturday'];
      
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin:  0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition:  0.4s;
}
#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}
main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75))
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color:#313131;
  font-size: 20px;
  
  appearance: none;
  border: none;
  outline: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  background-color: rgba(255,255,255,0.75);
  box-shadow: 0px 0px 16px rgba(0,0,0,0.25);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location{
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,0.25);
}
.location-box .date{
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
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
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(0,0,0,0.25);
  border-radius: 30px 0px;
  margin: 30px 0px;
  box-shadow:  3px 6px rgba(0,0,0,0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size:  48px;
  font-weight:  700;
  font-style: italic;
  text-shadow:  3px 6px rgba(0,0,0, 0.25);
}
.loader{
  position: fixed;
  left: 0px;
  top: 0px;
  width: 100%;
  height: 100%;
  z-index: 9999;
  background: url('//upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Phi_fenomeni.gif/50px-Phi_fenomeni.gif') 
              50% 50% no-repeat rgb(249,249,249);
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.hide{
  display: none;
}
.show{
  display: block;
}
</style>