<template>
  <div id="app">

    <main>

      <div class="container mt-5">
        <div class="row">

            <div class="col-md-6 offset-md-3">

                <h1 class="text-center mb-4">Travel Assistance Web-App</h1>

                <div class="input-group mb-3">
                    <input 
                      type="text" 
                      class="form-control" 
                      placeholder="Enter destination city" 
                      id="cityInput"
                      v-model="query"
                    >
                    <button @click="fetchWeatherInfo" class="btn btn-primary" type="button">Search</button>
                </div>

                <div v-if="(typeof weather.main != 'undefined')">
                  <h4 class="text-center py-3">{{ weather.name }}, {{ weather.sys.country }}</h4>
                </div>

                <div id="resultContainer">

                  <CityInformationCard :population="cityInfo.population" :gdpPerCapita="cityInfo.gdpPerCapita" v-if="(typeof cityInfo.population != 'undefined')"/>

                  <WeatherForecastCard :temp="weather.main['temp']" :humidity="weather.main['humidity']" :temp_min="weather.main['temp_min']" :temp_max="weather.main['temp_max']" v-if="(typeof weather.main != 'undefined')"/>

                  <ExchangeRateCard :code="exchange.code" :symbol="exchange.symbol" :result="exchange.result" v-if="(typeof exchange.code != 'undefined')"/>

                </div>

            </div>

        </div>
    </div>

    <DivFooter/>

    </main>

  </div>
</template>

<script>
import CityInformationCard from './components/Cards/CityInformationCard.vue';
import ExchangeRateCard from './components/Cards/ExchangeRateCard.vue';
import WeatherForecastCard from './components/Cards/WeatherForecastCard.vue';

import DivFooter from './components/Layout/DivFooter.vue';

export default {
  name: 'App',
  components: {
    CityInformationCard,
    ExchangeRateCard,
    WeatherForecastCard,

    DivFooter
  },
  data () {
    return {
      url_base2: 'https://api.worldbank.org/v2/',
      query: '',
      cityInfo: {},
      weather: {},
      exchange: {},
    }
  },
  methods: {
    fetchWeatherInfo () {
      if (this.query != "")
      {
        // Fetch the Weather
        const weather_url_base = 'https://api.openweathermap.org/data/2.5/';
        const weather_api_key = 'b5f3944288dbb3674ab81ed9370d9eb5';
        fetch(`${weather_url_base}weather?q=${this.query}&units=metric&APPID=${weather_api_key}`)
        .then(res => res.json())
        .then(data => {
          // Weather APi Result
          this.setWeatherResults(data);
          this.fetchCityPopulationInfo(data);
          this.fetchCityPerCapitaInfo(data);

          this.fetchCurrency(data.sys.country);
        });
      } else {
        alert('Preencha o campo');
      }
    },

    fetchCityPopulationInfo (data) {
      // Fetch the Country current population
      const country = data.sys.country; 
      const country_url_base = `https://api.worldbank.org/v2/country/${country}/indicator/SP.POP.TOTL?format=json&per_page=1`;
      fetch(country_url_base)
      .then(res => res.json())
      .then(data => {
        // Country APi Result
        this.setCityPopulationResults(data);
      });
    },

    fetchCityPerCapitaInfo (data) {
      // Fetch the Country current population
      const country = data.sys.country; 
      const country_url_base = `https://api.worldbank.org/v2/country/${country}/indicator/NY.GDP.PCAP.CD?format=json&per_page=1`;
      fetch(country_url_base)
      .then(res => res.json())
      .then(data => {
        // Country APi Result
        this.setCityPerCapitaResults(data);
      });
    },

    fetchCurrency (data) {
      // Fetch the Currency
      const currency_url_base = `https://restcountries.com/v2/alpha/${data}`;
      fetch(currency_url_base)
      .then(res => res.json())
      .then(data => {
        // Currency APi Result
        this.setCurrencyResults(data);
        this.fetchExchange(data.currencies[0].code);
      });
    },

    
    fetchExchange (data) {
      // Fetch the Exchange
      const exchange_url_base = `https://v6.exchangerate-api.com/v6/28b1bc46f2e08294c751f42b/pair/${data}/MZN`;
      fetch(exchange_url_base)
      .then(res => res.json())
      .then(data => {
        // Currency APi Result
        this.setExchangeResults(data);
      });
    },


    setWeatherResults (results) {
      this.weather = results;
    },

    setCityPopulationResults (results) {
      const data = results[1];
      this.cityInfo.population = data[0].value;
    },

    setCityPerCapitaResults (results) {
      const data = results[1];
      this.cityInfo.gdpPerCapita = data[0].value;
    },

    setCurrencyResults (results) {
      this.exchange = results.currencies[0];
    },

    setExchangeResults (results) {
      this.exchange.result = results["conversion_rate"];
    },

  }
}
</script>

<style>
body {
    font-family: 'Poppins', sans-serif;
    background-color: #f8f9fa;
    margin-bottom: 60px; 
}
h1 {
    font-weight: 900;
    background: linear-gradient(to right, #007bff, #00a8ff);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}
h4 {
  color: #999999;
  font-size: 1rem;
}
.footer {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 60px;
    background-color: #f8f9fa;
    text-align: center;
    line-height: 60px;
    color: #999999;
}
</style>
