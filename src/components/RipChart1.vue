<template>
  <div>
    <VueApexCharts
      foreColor="black"
      width="850"
      height="250"
      type="bar"
      :options="ChartOptions"
      :series="series"
    ></VueApexCharts>
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
      days: "53",
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
            type: "vertical",
            shadeIntensity: 1,
            opacityFrom: 1,
            opacityTo: 1,

            colorStops: [
              {
                offset: 10,
                color: "#a5b8d0",
                opacity: 1
              },
              {
                offset: 90,
                color: "#34558b",
                opacity: 1
              }
            ]
          }
        },
        dataLabels: {
          enabled: false
        },

        xaxis: {
          categories: [],
          labels: {
            show: true,
            rotate: -60,
            rotateAlways: false,
            hideOverlappingLabels: true,
            showDuplicates: false,
            trim: true,
            minHeight: undefined,
            maxHeight: undefined,
            style: {
              colors: "#235281",
              fontSize: "10px",
              fontFamily: '"Avenir",Helvetica, Arial, sans-serif',
              cssClass: "apexcharts-xaxis-label"
            }
          }
        },
        yaxis: {
          labels: {
            style: {
              color: "#235281",
              fontSize: "10px",
              fontFamily: '"Avenir",Helvetica, Arial, sans-serif',
              cssClass: "apexcharts-yaxis-label"
            }
          }
        }
      },
      series: [
        {
          name: "New Accounts Created by Week",
          data: []
        }
      ]
    };
  },
  mounted() {
    window;
    //rippled Lookup all stats
    fetch(this.url + "stats?descending=true&interval=week&limit=" + this.days)
      .then(pp => {
        return pp.json();
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
        date = moment(date).format("DD MMM YY");
        this.ChartOptions.xaxis.categories.push(date);

        var stat = this.data.stats[i].metric.accounts_created;
        this.series[0].data.push(stat);
      }
      this.ChartOptions.xaxis.categories.reverse();
      this.series[0].data.reverse();
    },
    yas() {
      var i;
      for (i = 0; i < this.days; i++) {
        var stat = this.data.stats[i].metric.accounts_created;
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