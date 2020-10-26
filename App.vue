<template>
  <div id="app">
    <div id="subcontainer">
      <input
        id="cityname"
        type="text"
        placeholder="Enter the name of the city"
        v-model="cityname"
      />
      <button id="btngetanalytics" v-on:click="getanalytics">
        Get Analystics
      </button>
    </div>
    <div id="secondsubcontainer">
      <p v-if="flag == true">Temperature: {{ temperature }} Celsius</p>
      <p v-if="flag == true">Temperature Max: {{ maxTemperature }} Celsius</p>
      <p v-if="flag == true">Temperature Min: {{ minTemperature }} Celsius</p>
      <p v-if="flag == true">Humidity: {{ humidity }} Percent</p>
      <p v-if="flag == true">Pressure: {{ pressure }} Hpa</p>
      <p v-if="flag == true">WindSpeed: {{ windSpeed }} Km/h</p>
      <p v-if="flag == false">Data from the Server will be Shown here</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function() {
    return {
      flag: false,
      cityname: "",
      temperature: 0,
      humidity: 0,
      minTemperature: 0,
      maxTemperature: 0,
      pressure: 0,
      windspeed: 0
    };
  },
  methods: {
    getanalytics: function() {
      let self = this;
      let checker = true;
      let url =
        "http://api.openweathermap.org/data/2.5/weather?q=" +
        this.cityname +
        "&units=metric&APPID=f536a43b87281796444519d7958f52ec";
      fetch(url)
        .then(function(response) {
          if (response.status == 200) {
            return response.json();
          } else {
            alert("Please enter correct City name");
            checker = false;
          }
        })
        .then(function(data) {
          if (checker == true) {
            self.flag = true;
            console.log(data);
           self.temperature = data["main"]["temp"];
           self.maxTemperature = data["main"]["temp_max"];
           self.minTemperature = data["main"]["temp_min"];
           self.humidity = data["main"]["humidity"];
           self.pressure = data["main"]["pressure"];
            self.windSpeed = data["wind"]["speed"];
          }
        });
      console.log(this.cityname);
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#cityname {
  height: 60px;
  width: 200px;
  text-align: center;
  font-size: 17px;
}
#btngetanalytics {
  margin-left: 12px;
  font-size: 12px;
  height: 60px;
  background-color: aqua;

}
#btngetanalytics:hover {
  background-color: aliceblue;
}
#secondsubcontainer {
  margin: 0 auto;
  margin-top: 80px;
  height: 500px;
  width: 500px;
  border: 1px solid;
}
</style>
