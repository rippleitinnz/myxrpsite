<template>
  <div>
    <div id="container" style="width:100%; height:400px;"></div>
  </div>
</template>








<script>
export default {
  data() {
  return {
    title: '',
    points: [10, 0, 8, 2, 6, 4, 5, 5],
    chartType: 'Spline',
    seriesColor: '#6fcd98',
    colorInputIsSupported: null,
    chartOptions: {
      chart: {
        type: 'spline'
      },
      title: {
        text: 'Sin chart'
      },
      series: [{
        data: [10, 0, 8, 2, 6, 4, 5, 5],
        color: '#6fcd98'
      }]
    }
  }
},
}
</script>
