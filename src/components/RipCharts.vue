<template>

      <div style= "bottom: 50px; left: 550px;">
      <VueApexCharts width="800" height="200" type="bar" :options="options" :series="series"></VueApexCharts>
    </div>

</template>

<script>

// charts
import VueApexCharts from "vue-apexcharts";

export default {

  name: "Vue Chart",
  components: { VueApexCharts },
 
  data: function() {
    return {
      //chart
      options: {
        chart: {
          id: "vuechart-example"
        },
        xaxis: {
          categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998]
        }
      },
      series: [
        {
          name: "series-1",
          data: [10, 20, 30, 40, 50, 60, 70, 90]
        }
      ],
    };
  }, 

  mounted() {
   
    //rippled Lookup all stats
    window
      .fetch("https://data.ripple.com/v2/stats?descending=true&limit=100")
      .then(p => {
        return p.json();
      })
      .then(data => {
        // Set it to the Vue App data
        this.xrpMetrics = data;
      });

   
  },

 
};
</script>

