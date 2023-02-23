<script>
import {
  evaluate, format
} from 'mathjs'

export default {
  data () {
    return {
      title: 'Ratio Calc',
      ratio: 2,
      start: 0,
      count: 100
    }
  },

  computed: {
    results () {
      const r = []
      let c = 0
      for (let i = this.start; i <= this.count; i++) {
        const s = '' + i + '*' + String(this.ratio)
        c = format(evaluate(s), { precision: 14 })
        r.push({ i, c })
      }
      return r
    }
  }
}
</script>

<template lang="pug">
header
  h1 {{ title }}

main

  section
    form()

      span.field
        label(for="start") Start
        input(type="text" v-model="start" id="start")

      span.field
        label(for="ratio") Ratio
        input(type="text" v-model="ratio" id="ratio")

      span.field
        label(for="count") Count
        input(type="text" v-model="count" id="count")

  section
    h2 Results
      ul.results
        li(v-for="result of results")
          span.i {{ result.i  }}
          span.r x {{ ratio }}
          span.c {{ result.c  }}

footer
  p &copy snaz4d kkthxbye
</template>

<style scoped>
label {
  display: block
}

.field {
  display: inline-block
}

ul {
  list-style-type: none;
  padding: 0;
}

section {
  margin-bottom: 48px
}

li {
  font-size: 12px;
  display: flex;
  flex-flow: row nowrap;
}

li:nth-child(2n) {
  background-color: #fafafa;
}

li span {
  min-width: 80px;
}

.c {
  margin-left: auto;
  font-weight: bold;
  text-align: right;
}

.r {
  margin: 0 auto;
}
</style>
