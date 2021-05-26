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
      gradient: null,
      gradient2: null,
    };
  },
  mounted() {
    this.gradient = this.$refs.canvas
      .getContext("2d")
      .createLinearGradient(0, 0, 0, 450);
    this.gradient2 = this.$refs.canvas
      .getContext("2d")
      .createLinearGradient(0, 0, 0, 450);

    this.gradient.addColorStop(0, "rgba(255, 0,0, 0.5)");
    this.gradient.addColorStop(0.5, "rgba(255, 0, 0, 0.25)");
    this.gradient.addColorStop(1, "rgba(255, 0, 0, 0)");

    this.gradient2.addColorStop(0, "rgba(0, 231, 255, 0.9)");
    this.gradient2.addColorStop(0.5, "rgba(0, 231, 255, 0.25)");
    this.gradient2.addColorStop(1, "rgba(0, 231, 255, 0)");

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
                borderColor: "#05CBE1",
                pointBackgroundColor: "white",
                pointBorderColor: "white",
                borderWidth: 1,
                backgroundColor: this.gradient2,
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
