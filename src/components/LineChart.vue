<script>
  import {Line, mixins} from 'vue-chartjs'
  import ChartJSdragDataPlugin from 'chartjs-plugin-dragdata'

  const { reactiveProp } = mixins

  export default Line.extend({
    mixins: [reactiveProp],
    data () {
      return {
        options: {
          scales: {
            yAxes: [{
              ticks: {
                max: 1,
                min: 0
              },
              gridLines: {
                display: true
              }
            }],
            xAxes: [ {
              gridLines: {
                display: false
              }
            }]
          },
          legend: {
            display: true
          },
          responsive: true,
          maintainAspectRatio: false,
          dragData: true,
          dragX: false,
          onDragStart: function (event, element) {
          },
          onDrag: function (event, datasetIndex, index, value) {
          },
          onDragEnd: function (event, datasetIndex, index, value) {
            console.log(datasetIndex + ' ' + index + ' ' + value)
          }
        }
      }
    },
    mounted () {
      this.addPlugin(ChartJSdragDataPlugin)
      // this.chartData is created in the mixin
      this.renderChart(this.chartData, this.options)
    }
  })
</script>
