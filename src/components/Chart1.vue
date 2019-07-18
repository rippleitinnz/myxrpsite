
<template>
  <div class="chartElem">
    <div class="row">
      <highcharts class="chart" :options="chartOptions" :updateArgs="updateArgs"></highcharts>
      <div>
      
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: "This Chart",
      points: [],
      chartType: "Spline",
      seriesColor: "#6fcd98",
      colorInputIsSupported: null,
      animationDuration: 1000,
      updateArgs: [true, true, { duration: 1000 }],
      chartOptions: {
        chart: {
          type: "spline"
        },
        title: {
          text: "New Accounts"
        },
        series: [
          {
            data: [],
            color: "#6fcd98"
          }
        ]
      }
    };
  },


  mounted() {
    window;
    //rippled Lookup all stats
    fetch(this.url + "stats?descending=true&interval&limit=" + this.days)
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

  created() {
    let i = document.createElement("input");
    i.setAttribute("type", "color");
    i.type === "color"
      ? (this.colorInputIsSupported = true)
      : (this.colorInputIsSupported = false);
  }
};


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







<style scoped>
input[type="color"]::-webkit-color-swatch-wrapper {
  padding: 0;
}
#colorPicker {
  border: 0;
  padding: 0;
  margin: 0;
  width: 30px;
  height: 30px;
}
.numberInput {
  width: 30px;
}
</style>

