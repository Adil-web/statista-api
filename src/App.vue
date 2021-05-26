<template>
  <div id="app">
    <AreaChart :years="usaData.years" :population="usaData.population" />
    <LineChart :years="usaData.years" :population="usaData.population" />
    <BarChart :years="usaData.years" :population="usaData.population" />
    <PieChart />
  </div>
</template>

<script>
import AreaChart from "./components/AreaChart.vue";
import PieChart from "./components/PieChart.vue";
import BarChart from "./components/BarChart.vue";
import LineChart from "./components/LineChart.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    AreaChart,
    PieChart,
    BarChart,
    LineChart,
  },
  data() {
    return {
      usaData: {},
      chartData: {
        Books: 24,
        Magazine: 30,
        Newspapers: 10,
      },
      localData: ''
    };
  },
  beforeCreate() {
    this.localData = localStorage.getItem('statistics')
        console.log(JSON.parse(this.localData))

    
    if(!this.localData) {
    axios
      .get("http://localhost:5000/api/statistics")
      .then((json) => {
      
      localStorage.setItem('statistics', JSON.stringify(json.data.items))
        console.log(json.data)
      })
    }
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
