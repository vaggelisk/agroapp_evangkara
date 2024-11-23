<template>
  <v-container >
    <v-responsive
      class="align-centerfill-height mx-auto"
      max-width="900"
    >

      <v-card class="py-4">
        <div class="text-center">
          <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>
          <h1 class="text-h3 font-weight-bold">Weather Dashboard</h1>
        </div>
      </v-card>


      <div class="py-4" />

      <div class="py-2">
        <v-btn class="mr-2" variant="tonal" @click="setNowValues">
          Now
        </v-btn>
        <v-btn class="ma-2" variant="tonal" @click="setTodayValues">
          Today
        </v-btn>
        <v-btn  class="ma-2" variant="tonal" @click="openDateModal">
          Date
        </v-btn>
      </div>

      <v-row>
        <v-col>
          <v-dialog max-width="350px" persistent v-model="modal">
            <v-card>
              <v-date-picker
                v-model="date" scrollable actions
                :allowed-dates="allowedDates"
                min="2024-11-10"
                max="2024-12-10"
              >
              </v-date-picker>
              <v-card-actions>
                <v-btn variant="outlined" color="primary" @click="modal=false">Cancel</v-btn>
                <v-btn variant="elevated" color="primary"  @click="closeDateModal(true)">OK</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-col>
      </v-row>
      {{this.time==='today' ? 'Today' : date}}
      <v-card class="py-4" rounded="lg">
        <template #image>
          <v-img :src=iconUrl position="bottom right" />
        </template>
        <template #title >
          <h2 class="py-4 text-h5 font-weight-bold">{{ temperature }}°C</h2>
        </template>
        <template #subtitle>
          <div class="text-subtitle-1">
            {{main}} - {{mainDescription}}
          </div>
        </template>

        <v-overlay
          opacity="0.12"
          scrim="white"
          contained
          model-value
          persistent
        />
      </v-card>

      <v-card class="py-4 mt-2">

      <v-row>

        <v-col cols="4">
          <v-card
            class="py-4"
            rel="noopener noreferrer"
            rounded="lg"
            variant="outlined"
          >
            <template #image>
              <v-img position="top right" />
            </template>
            <template #title>
              <span class="text-h5 font-weight-bold">{{ tempFeelsLike }}°C</span>
            </template>
            <template #subtitle>
              <div class="text-subtitle-1">
               Feels like
              </div>
            </template>
            <v-overlay
              opacity=".06"
              scrim="white"
              contained
              model-value
            />
          </v-card>
        </v-col>

        <v-col cols="4">
          <v-card
            class="py-4"
            rel="noopener noreferrer"
            rounded="lg"
            variant="outlined"
          >
            <template #image>
              <v-img position="top right" />
            </template>
            <template #title>
              <span class="text-h5 font-weight-bold">{{ windSpeed }}</span><span class="text-subtitle-1"> m/s</span>
            </template>
            <template #subtitle>
              <div class="text-subtitle-1">
               Wind
              </div>
            </template>
            <v-overlay
              opacity=".06"
              scrim="white"
              contained
              model-value
            />
          </v-card>
        </v-col>
        <v-col cols="4">
          <v-card
            class="py-4"
            color="surface-variant"
            rel="noopener noreferrer"
            rounded="lg"
            variant="outlined"
          >
            <template #image>
              <v-img position="top right" />
            </template>
            <template #title>
                <span class="text-h5 font-weight-bold">{{ windGust }}</span><span class="text-subtitle-1"> m/s</span>
            </template>
            <template #subtitle>
              <div class="text-subtitle-1">
               Wind Gust
              </div>
            </template>
            <v-overlay
              opacity=".06"
              scrim="white"
              contained
              model-value
            />
          </v-card>
        </v-col>
        <v-col cols="4">
          <v-card
            class="py-4"
            color="surface-variant"
            rel="noopener noreferrer"
            rounded="lg"
            variant="outlined"
          >
            <template #image>
              <v-img position="top right" />
            </template>
            <template #title>
              <span class="text-h5 font-weight-bold">{{ windDeg }}°</span>
            </template>
            <template #subtitle>
              <div class="text-subtitle-1">
                Wind Deg
              </div>
            </template>
            <v-overlay
              opacity=".06"
              scrim="white"
              contained
              model-value
            />
          </v-card>
        </v-col>

        <v-col cols="4">
          <v-card
            class="py-4"
            color="surface-variant"
            rel="noopener noreferrer"
            rounded="lg"
            variant="outlined"
          >
            <template #image>
              <v-img position="top right" />
            </template>
            <template #title>
              <span class="text-h5 font-weight-bold">{{ humidity }}</span><span class="text-subtitle-1">%</span>
            </template>
            <template #subtitle>
              <div class="text-subtitle-1">
                Humidity
              </div>
            </template>
            <v-overlay
              opacity=".06"
              scrim="white"
              contained
              model-value
            />
          </v-card>
        </v-col>

        <v-col cols="4">
          <v-card
            class="py-4"
            color="surface-variant"
            rel="noopener noreferrer"
            rounded="lg"
            variant="outlined"
          >
            <template #image>
              <v-img position="top right" />
            </template>
            <template #title>
              <span class="text-h5 font-weight-bold">{{ pressure }}</span><span class="text-subtitle-1">hPa</span>
            </template>
            <template #subtitle>
              <div class="text-subtitle-1">
                Pressure
              </div>
            </template>
            <v-overlay
              opacity=".06"
              scrim="white"
              contained
              model-value
            />
          </v-card>
        </v-col>

      </v-row>
      </v-card>

      <v-card class="py-2 my-4">
        <v-row class="py-8">
          <v-col cols="12">
            <Chart :daily="dailyForecast" />
          </v-col>
        </v-row>
      </v-card>

    </v-responsive>
  </v-container>
</template>

<script>
import axios from "axios";
import Chart from "@/components/Chart.vue";


const apikey = "feff206daa60b539abe8fae8f2ab7f29";

export default {
  name: 'TemperatureDashboard',
  components: {Chart},
  data() {
    return {
      city: "",
      serialDay: null,
      time: "now",
      date: new Date(),
      selectedDate: new Date(),
      modal: false,
      weatherData: null,
      dailyForecast: [],
    };
  },
  computed: {
    temperature() {
      if (this.time==='now') {
        return this.weatherData
          ? Math.floor(this.weatherData.main.temp - 273)
          : null;
      } else if (this.time==='today') {
        return this.dailyForecast[0]
          ? this.dailyForecast[0].temp
          : null
      } else if (this.time==='1' || this.time==='2' || this.time==='3' || this.time==='4') {
        return this.dailyForecast[Number(this.time)]
          ? this.dailyForecast[Number(this.time)].temp
          : null
      } else {
        return null
      }
    },
    main() {
      return this.weatherData
        ? this.weatherData.weather[0].main
        : null;
    },
    mainDescription() {
      return this.weatherData
        ? this.weatherData.weather[0].description
        : null;
    },
    tempFeelsLike() {
      return this.weatherData
        ? Math.floor(this.weatherData.main.feels_like - 273)
        : null;
    },
    windSpeed() {
      return this.weatherData
        ? this.weatherData.wind.speed
        : null;
    },
    windGust() {
      return this.weatherData
        ? this.weatherData.wind.speed
        : null;
    },
    windDeg() {
      return this.weatherData
        ? this.weatherData.wind.deg
        : null;
    },
    humidity() {
      return this.weatherData
        ? this.weatherData.main.humidity
        : null;
    },
    pressure() {
      return this.weatherData
        ? this.weatherData.main.pressure / 1000
        : null;
    },
    iconUrl() {
      return this.weatherData
        ? `https://api.openweathermap.org/img/w/${this.weatherData.weather[0].icon}.png`
        : null;
    },
  },
  mounted() {
    this.fetchCurrentLocationWeather();
  },
  methods: {
    allowedDates(val) {
      let today = new Date()
      let fiveDaysLater = new Date();
      fiveDaysLater.setDate(today.getDate() + 4);
      return val > today && val < fiveDaysLater;
    },
    setTodayValues() {
      this.time = 'today'
    },
    setNowValues() {
      this.time = 'now'
      this.date = new Date()
    },
    closeDateModal(save) {
      if (save) {
        let today = new Date()
        let diafora = this.date - today
        this.serialDay = Math.floor( diafora / (24 * 60 * 60 * 1000) + 1 )
        this.time = this.serialDay.toString()
      }
      this.modal = false
    },
    openDateModal() {
      this.modal = true;
    },
    async fetchCurrentLocationWeather() {
      const url = `https://api.openweathermap.org/data/2.5/weather?lat=40.58725980318928&lon=22.948223362612612&appid=${apikey}`;
      await this.fetchWeatherData(url);
    },
    async fetchWeatherData(url) {
      try {
        const response = await axios.get(url);
        this.weatherData = response.data;
        // Fetch forecast data
        await this.fetchForecast();
      } catch (error) {
        console.error("Error fetching weather data:", error);
      }
    },
    async fetchForecast() {
      const urlcast = `https://api.openweathermap.org/data/2.5/forecast?q=Kalamaria&cnt=56&appid=${apikey}`;
      try {
        const response = await axios.get(urlcast);
        const forecast = response.data;
        this.dayForecast(forecast);
      } catch (error) {
        console.error("Error fetching forecast data:", error);
      }
    },
    dayForecast(forecast) {
      this.dailyForecast = [];
      for (let i = 0; i < forecast.list.length; i += 8) {
        const date = new Date(forecast.list[i].dt * 1000);
        let sumTemp     = 0
        let sumTemp_max = 0
        let sumTemp_min = 0
        for (let j=i; j < i+8; j += 1) {
          sumTemp     +=  forecast.list[j].main.temp
          sumTemp_max +=  forecast.list[j].main.temp_max
          sumTemp_min +=  forecast.list[j].main.temp_min
        }
        this.dailyForecast.push({
          date: date.toDateString(undefined, "Europe/Athens"),
          temp: Math.floor(sumTemp/8 - 273),
          temp_max: Math.floor(sumTemp_max/8 - 273),
          temp_min: Math.floor(sumTemp_min/8 - 273),
          description: forecast.list[i].weather[0].description,
        });
      }
    },
  },
};

</script>
