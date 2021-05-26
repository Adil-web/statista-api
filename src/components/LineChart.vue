<script>
import { Line } from "vue-chartjs";
import axios from "axios";

export default {
  props: {
    years: {
      type: Array,
    },
    population: {
      type: Array,
    },
  },
  extends: Line,
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
                label: "USA Population",
                data: this.usaData.population,
                backgroundColor: "transparent",
                borderColor: "rgba(1, 116, 188, 0.50)",
                pointBackgroundColor: "rgba(171, 71, 188, 1)",
              },
            ],
          },
          {
            responsive: true,
            maintainAspectRatio: false,
            title: {
              display: true,
              text: "My Data",
            },
          }
        );
      })
      .catch((err) => console.log(err));
  },
};
</script>