<<template>
  <div id="app">
  <main>
    <div class="search-box">
      <input type="text"
       class="search-bar"
        placeholder="Search..."
        v-model="query"
        @keypress="fetchWeather"
        />
    </div>
    <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
      <div class="location-box">
        <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
        <div class="date">{{dateCreator()}}</div>
      </div>

      <div class="weather-box">
        <div class="temperature">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather-status">{{weather.weather[0].main}}</div>
      </div>
    </div>
  </main>
  </div>
</template>

<script>

import axios from "axios"

export default {
  name: 'app',
  data(){
    return {
      api_key :'f401be24a6ae97c3177533197510126c',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{}

    }
  },
  methods: {
        async fetchWeather(e) {
        if (e.key == "Enter") {
        let response = await axios.get(
        `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        );
        this.setResults(response.data);
        }
        },
        setResults(returnedResponse) {
        this.weather = returnedResponse;
        },
    dateCreator () {
      let dates = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[dates.getDay()];
      let date = dates.getDate();
      let month = months[dates.getMonth()];
      let year = dates.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}

</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

#app{
  background-image: url('./assets/forecast.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}

main{
  min-height: 100vh;
  padding: 20px;

}

.search-box{
  width: 100%;
  margin-bottom: 20px;
}

.search-bar{
display: block;
width: 100%;
padding: 10px;
font-size: 25px;

appearance: none;
border: none;
outline: none;
background-color:rgb(169, 169, 169, 0.85);
border-radius:15px 15px 15px 15px;
transition: 0.5s;
}

.search-bar:focus{
  border-radius: 5px 5px 5px 5px;

}

.location{
font-size: 40px;
font-weight: 600;
text-align: center;
}
.date{
  font-size: 20px;
font-weight: 300;
text-align: center;
font-style: italic;
}

.weather-box{
  text-align: center;
}

.temperature{
  display: inline-block;
  padding: 20px;
  color: aliceblue;
  font-size: 150px;
  font-weight: 2000;
  
}

.weather-status{
  color: aliceblue;
  font-size: 50px;
}

</style>
