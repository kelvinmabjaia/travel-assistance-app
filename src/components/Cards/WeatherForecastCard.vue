<template>
  <div class="card text-center mb-4">
    <div class="card-content">
      <h3 class="card-title">
          <i class="fas fa-cloud-sun icon"></i>Weather Forecast<div class="px-4"></div>
      </h3>
      <ul class="list-unstyled">
          <li>
            <h2>{{ temp }} °C</h2>
          </li>
          <li>
            <span class="font-weight-bold">Min: </span>
            <span>{{ temp_min }} °C</span>
            |
            <span class="font-weight-bold">Max: </span>
            <span>{{ temp_max }} °C</span>
          </li>
          <li>
            <i class="fas fa-tint icone"></i> <span>{{ humidity }}%</span>
          </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WeatherForecastCard',
  props: {
    temp: String,
    temp_min: String,
    temp_max: String,
    humidity: String
  }
};
</script>

<style>
.card {
    border: none;
    background-color: #fff;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
}
span {
  color: rgb(41, 41, 41);
}
.card-title {
    font-weight: 500;
    color: #00a8ff;
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 0;
    position: relative;
    padding-top: .25rem;
    padding-bottom: 1.25rem;
}
.icone {
font-size: 1rem;
color: #007bff;
}
.card-content {
    padding: 1rem;
}
.card-content > ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
}
.card-content > ul > li {
    margin-bottom: 0.5rem;
}
.card-title > span {
    position: absolute;
    bottom: -1.5rem;
    left: 0;
    right: 0;
    text-align: center;
    font-size: 0.75rem;
    color: #777;
}
</style>
