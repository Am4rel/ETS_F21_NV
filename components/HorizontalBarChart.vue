<script>
import { Prop, Vue, Watch } from 'vue-property-decorator'
import Component from 'nuxt-class-component'
import { HorizontalBar } from 'vue-chartjs'

import getRandomColors from '../utils/getRandomColors';

@Component({extends: HorizontalBar})
export default class Chart extends Vue {
  @Prop({type: Array, required: true}) dataLabels // Массив данных для оси координат Х, вида ['точка1', 'точка2', ...]. Сюда закидывать массив дат.
  @Prop({type: Array, required: true}) dataArray // Массив с данными для построение графика, вида [8,10,15, *остальные точки по оси Y*. Сюда закидывать массив значений для графика.
  @Prop({type: String, required: true}) datalabel // Массив данных для подписей линий графика, вида ['подпись1', 'подпись2', ...]. Сюда закидывать названия валют.
  @Prop({type: String}) chartTitle // Название графика.

  // Объект настроек для графика. В данном случае график будет менять размер при изменении размеров окна с сохранением пропорций,
  // отключена легенда и добавлены надписи внутри баров со значениями.
  options = {
        responsive: true,
        maintainAspectRatio: false,
        legend: {
          display: false
        },
        tooltips: {
            enabled: true
        },
        title: {
          display: false,
        },
        animation: {
          duration: 1000,
          onComplete: function () {
            const chartInstance = this.chart
            const ctx = chartInstance.ctx;
            ctx.textAlign = 'center';
            ctx.fillStyle = "rgba(111, 108, 153, 1)";
            ctx.textBaseline = 'bottom';
            this.data.datasets.forEach(function (dataset, i) {
                let meta = chartInstance.controller.getDatasetMeta(i);
                meta.data.forEach(function (bar, index) {
                    let maxData = bar._xScale._endValue
                    let breakValueForTitlePosition = (bar._xScale._endValue - bar._xScale._startValue) / 100 * 10

                    let data = `${dataset.data[index]}%`;
                    let yPosition = bar._model.y + 7
                    let xPosition = bar._model.x + 25

                    if (maxData - dataset.data[index] <= breakValueForTitlePosition){
                      xPosition = bar._model.x - 25
                    }

                    ctx.fillText(data, xPosition, yPosition);
                });
            });
          }
        }
      }

  mounted () {
    this.updateChart();
  }

  @Watch('dataLabels', {deep: true})
  @Watch('dataArray', {deep: true})
  @Watch('datalabel', {deep: true})
  updateChart() {
    const colors = getRandomColors(this.dataLabels.length, 0.25); // ставим рандомные цвета каждому бару
    
    if (this.chartTitle) {
      this.options.title.display = true
      this.options.title.text = this.chartTitle
    }

    this.renderChart({
      labels: this.dataLabels,
      datasets: [{
        data: this.dataArray,
        label: this.datalabel,
        backgroundColor: colors,
        borderRadius: 5,
        borderSkipped: false,
      }]},
      this.options
    )
  }
}
</script>