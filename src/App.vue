<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
    :style="{ backgroundImage: `url(${backgroundImage})` }"
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search"
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
            <img
              v-if="weather.sys.country"
              :src="getCountryFlag(weather.sys.country)"
              alt="Country Flag"
              class="flag"
            />
            <div class="date">{{ dateBuilder() }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      api_key: "89763174adcc06b15a1649d4c0288a17",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      backgroundImages: [
        "https://www.pixelstalk.net/wp-content/uploads/2016/08/Background-Images-Full-HD-Wallpapers.jpg",
        "https://wallpapercave.com/wp/wp2570989.jpg",
        "https://images.pexels.com/photos/709552/pexels-photo-709552.jpeg?cs=srgb&dl=pexels-samandgos-709552.jpg&fm=jpg",
        "https://images.pexels.com/photos/440731/pexels-photo-440731.jpeg?cs=srgb&dl=pexels-akos-szabo-145938-440731.jpg&fm=jpg",
      ],
      backgroundImage: "",
    };
  },

  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => res.json())
          .then((results) => {
            this.setResults(results);
            this.changeBackgroundImage();
          });
      }
    },
    setResults(results) {
      this.weather = results;
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
    getCountryFlag(countryCode) {
      return `https://flagcdn.com/w320/${countryCode.toLowerCase()}.png`;
    },
    changeBackgroundImage() {
      const randomIndex = Math.floor(
        Math.random() * this.backgroundImages.length
      );
      this.backgroundImage = this.backgroundImages[randomIndex];
    },
  },
  mounted() {
    // Set an initial random background image when the app loads
    this.changeBackgroundImage();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", serif;
}
#app {
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
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  position: relative;
}

.location-box .location .flag {
  display: block;
  margin: 10px auto 0;
  width: 50px;
  height: auto;
  border-radius: 4px;
  box-shadow: 2px 4px rgba(0, 0, 0, 0.25);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-weight: 900;
  font-size: 102px;
  text-shadow: 3px 6px rgba(255, 255, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
