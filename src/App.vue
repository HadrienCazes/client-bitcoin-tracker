<template>
  <div id="app">
    <h2 class="title is-5">Bitcoin Price Trends</h2>
    <Bitcoin v-if="loaded" :chartdata="chartdata" :options="options"></Bitcoin>
  </div>
</template>

<script>
import Bitcoin from './components/Bitcoin';
import axios from 'axios';
import moment from 'moment';

export default {
  name: 'app',
  components: {
    Bitcoin
  },
  data: () => ({
    labels: [],
    data: [],
    chartdata: null,
    options: null,
    loaded: false
  }),
  async mounted () {
  this.loaded = false;
  axios.get('http://localhost:3000/data')
  .then((res) => {
    for (var i in res.data) {
      this.labels.push(moment(Date(res.data[i].Date)).format('MMMM Do YYYY, h:mm:ss a'));
      this.data.push(res.data[i].Weighted_Price);
    }

    this.chartdata = {
      labels : this.labels,
      datasets: [
        {
          label: 'Bitcoin price',
          data: this.data
        }
      ]
    }


    this.options = {
      scales: {
        yAxes: [{
          ticks: {
            beginAtZero: true
          }
        }]
      }
    }

    this.loaded = true
  })
  .catch((err) => {
    console.log(err);
  })
}
}
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
</style>
