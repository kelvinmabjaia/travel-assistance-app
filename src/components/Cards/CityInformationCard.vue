<template>
  <div class="card text-center mb-4">
    <div class="card-content">
      <h3 class="card-title">
        <i class="fas fa-info-circle icon"></i>City Information<div class="px-4"></div>
      </h3>
      <ul class="list-unstyled">
        <li class="city">
          <i class="fas fa-user icone"></i> <span>{{ population.toLocaleString() }}</span>
        </li>
        <li><span class="font-weight-bold"> GDP per Capita: ${{ convert(this.gdpPerCapita) }}</span> <span id="localCurrency"></span></li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CityInformationCard',
  props: {
    population: String,
    gdpPerCapita: String
  },
  data() {
    return {
      convertedString: "",
    };
  },
  methods: {
    convert(value) {
      const floatValue = Number(value);
      return floatValue.toFixed(2);
    },
  },
};
</script>

<style>
.card {
    border: none;
    background-color: #fff;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
}
li.city{
  font-size: 1.4rem;
}
li.city > i {
  font-size: 1.3rem;
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
.icon {
font-size: 2rem;

background-color: #e3f2fd;
border-radius: 25%;
padding: 0.5rem;
margin-right: 1.5rem;
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
