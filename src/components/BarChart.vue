<script>
import { Bar } from "vue-chartjs";
import axios from "axios";

export default {
  extends: Bar,
  props: {
    years: {
      type: Array,
    },
    population: {
      type: Array,
    },
  },
  data() {
    return {
      usaData: {},
    };
  },
  mounted() {
    axios
      .get("https://datausa.io/api/data?drilldowns=Nation&measures=Population")
      .then((json) => {
        this.usaData.years = [];
        this.usaData.population = [];
        json.data.data.map((e) => {
          this.usaData.years.unshift(e.Year);
          this.usaData.population.unshift(e.Population);
        });
        this.renderChart(
          {
            labels: this.usaData.years,
            datasets: [
              {
                label: "USA populations",
                backgroundColor: "#f87979",
                data: this.usaData.population,
              },
            ],
          },
          { responsive: true, maintainAspectRatio: false }
        );
      })
      .catch((err) => console.log(err));
  },
};
</script>
