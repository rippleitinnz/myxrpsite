<template>
  <div>
    <VueApexCharts width="850" height="250" type="bar" :options="ChartOptions" :series="series"></VueApexCharts>
  </div>
</template>


<script>
// charts
import VueApexCharts from "vue-apexcharts";
import * as moment from "moment";

export default {
  name: "VueChart",
  components: { VueApexCharts },

  data() {
    return {
      //chart
      url: "https://data.ripple.com/v2/",
      days: "45",
      data: {
        Xaxis: [],
        Yaxis: []
      },
      ChartOptions: {
        chart: {
          id: "vuechart-example",
          toolbar: {
            show: false
          }
        },
        stroke: {
          show: true,
          curve: "smooth",
          lineCap: "butt",
          colors: undefined
        },
        fill: {
          type: "gradient",
          gradient: {
            shade: "dark",
            type: "horizontal",
            shadeIntensity: 0.7,
            gradientToColors: undefined, // optional, if not defined - uses the shades of same color in series
            inverseColors: true,
            opacityFrom: 1,
            opacityTo: 1,
            stops: [0, 75, 100],
            colorStops: []
          }
        },
        dataLabels: {
          enabled: false
        },

        xaxis: {
          categories: [],
          labels: {
            tickAmount: 8,
            hideOverlappingLabels: true
          }
        }
      },
      series: [
        {
          name: "New Accounts Created",
          data: []
        }
      ]
    };
  },
  mounted() {
    window;
    //rippled Lookup all stats
    fetch(this.url + "stats?descending=true&limit=" + this.days)
      .then(response => {
        return response.json();
      })

      .then(json => {
        // Set it to the Vue App data
        this.data = json;
        console.log(json.result);
        this.axis();
      });
  },
  methods: {
    axis: function() {
      var days = parseInt(this.days, 10);
      var i;
      for (i = 0; i < days; i++) {
        var date = this.data.stats[i].date;
        date = moment(date).format("D MMM");
        this.ChartOptions.xaxis.categories.push(date);

        var stat = this.data.stats[i].metric.transaction_count;
        this.series[0].data.push(stat);
      }
      this.ChartOptions.xaxis.categories.reverse();
      this.series[0].data.reverse();
    },
    yas() {
      var i;
      for (i = 0; i < this.days; i++) {
        var stat = this.data.stats[i].metric.transaction_count;
        this.ChartOptions.series.data.push(stat);
        return stat;
      }
    }
  },
  computed: {
    Xaxis() {
      return null;
    },
    Yaxis() {
      return null;
    }
  }
};
</script>

