<template lang="pug">
header
  h1 {{ title }}

main
  section
    form
      span.field(v-for="field, key in rform" :class="key")
        label(:for="field") {{ key }}
        input(type="number" v-model="rform[key]" :id="field")

  section.results
    ul
      li(v-for="result, i of results")
        span.inc
          bar(:data="barData[i]" :options="chartOptions" class="bars")
          Line(:data="barData[i]" :options="chartOptions" class="line")
          pie(:data="barData[i]" :options="chartOptions" class="pie")
          radar(:data="barData[i]" :options="chartOptions" class="radar")
          PolarArea(:data="barData[i]" :options="chartOptions" class="polar")
          Doughnut(:data="barData[i]" :options="chartOptions" class="doughnut")


footer
  p &copy snaz4d kkthxbye
</template>

<script>
import {
  evaluate, format
} from 'mathjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  RadialLinearScale,
  LinearScale,
  LineElement,
  PointElement,
  ArcElement,
  Colors
} from 'chart.js'
import { Bar, Line, Pie, Doughnut, Bubble, PolarArea, Radar, Scatter } from 'vue-chartjs'

ChartJS.register(Tooltip, Colors, CategoryScale, LinearScale, RadialLinearScale, BarElement, PointElement, LineElement, ArcElement)

export default {
  data () {
    return {
      title: 'Ratio Visualizer',
      rform: {
        ratio: 1.61803398875,
        startingNumber: 33,
        displayResults: 7,
        showNextNumbers: 1
      },
      chartTypes: ['line', 'bar', 'pie', 'bubble', 'doughnut', 'radar', 'scatter', 'polarArea'],
      chartType: 'bar'
    }
  },

  components: {
    Bar,
    Line,
    Pie,
    Doughnut,
    Bubble,
    PolarArea,
    Radar,
    Scatter
  },

  watch: {
    results: (a, b, el) => console.log('cc', a, b, el)
  },

  computed: {
    results () {
      const r = []
      let c = 0
      const { startingNumber, ratio, showNextNumbers, displayResults } = this.rform
      const ii = (c) => format(evaluate(`${c} * ${ratio || 1}`), { precision: 8 })

      for (let i = 0; i < showNextNumbers; i++) {
        const n = Number(startingNumber || 0) + i
        let x = ii(n)
        const increments = [x]
        for (let j = 0; j < displayResults - 1; j++) {
          x = ii(x)
          increments.push(x)
        }
        r.push({ n, increments })
      }
      return r
    },

    barData () {
      return this.results.map(res => {
        const labels = res.increments
        labels.unshift(res.n)
        return {
          labels,
          datasets: [{ data: labels.map(i => Number(i)) }]
        }
      })
    },

    chartOptions () {
      return {
        responsive: true
      }
    }
  }
}
</script>



<style scoped lang="stylus">
header
main
footer
  padding 16px 20px
  width 100%

header
  flex 0 0

footer
  margin-top auto

main
  display flex
  flex-flow row wrap
  gap 20px
  margin-top 12px
  flex 1 0

label
  display block

input
select
  background-color black
  border 0
  outline 0
  padding 8px
  font-size 16px
  color #ccc
  border-radius 2px
  width 100%

section
  flex 1 1

h1
  font-weight bold
  font-size 32px
  line-height 36px

.ratio
  input
    font-weight bold


canvas
  flex 1 1 40%
  max-width 49%
  max-height 300px
  margin-bottom 32px

form
  display flex
  flex-flow row wrap
  gap 8px
  flex 0 1

.field
  display block
  flex 1 1 auto

  &:not(:last-child)
    margin-bottom 12px

ul
  list-style-type none
  padding 0

li
  font-size: 12px;
  display: flex;
  flex-flow: column nowrap;
  position: relative;
  text-align left

  &:not(:last-child)
    margin-bottom 8px

  span
    &:not(.inc)
      display flex
      flex-flow row nowrap
      margin-bottom 1px

.bar
  width 100%
  background blue
  order 0
  margin-right auto

.inc
  display: flex
  flex-flow row wrap
  gap 12px
  width 100%


.r
  margin: 0 auto;

.results
  flex 1 1 70%

</style>
