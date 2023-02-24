<script>
import {
  evaluate, format
} from 'mathjs'


export default {
  data () {
    return {
      title: 'Ratio Calc',
      rform: {
        ratio: 2,
        start: 0,
        count: 100,
        ratioCalc: 3
      }

    }
  },

  computed: {
    results () {
      const r = []
      let c = 0
      const { start, ratio, count, ratioCalc } = this.rform
      for (let i = 0; i < count; i++) {
        const n = Number(start || 0) + i
        const s = n + ' * ' + String(ratio || 1)
        const increments = []
        const c = format(evaluate(s), { precision: 8 })
        const ii = (c) => format(evaluate(`${c} * ${ratio}`), { precision: 8 })
        let x = c
        for (let j = 0; j < ratioCalc; j++) {
          x = ii(x)
          increments.push(x)
        }
        r.push({ n, c, increments })
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

      span.field(v-for="field, key in rform")
        label(:for="field") {{ key }}
        input(type="text" v-model="rform[key]" :id="field")

  section
    h2 Results
      ul.results
        li(v-for="result of results")
          span.bar
          span.i {{ result.n  }}
          span.c {{ result.c  }}
          span.inc
            span.c(v-for="inc of result.increments") {{ inc }}

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
  border-bottom: 1px solid #777;
  position: relative;
}

.bar {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

li span {
  min-width: 80px;
}
.inc {
  display: flex;
  flex-flow: row nowrap;
}
span.c {
  margin-left: 10px;
  font-weight: bold;
  text-align: right;
  width: 100px;
}

.r {
  margin: 0 auto;
}
</style>
