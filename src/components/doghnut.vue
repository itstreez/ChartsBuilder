<template>
  <div id="test">
    <b-button
      class="btn btn-light bg-white rounded-pill shadow-sm px-4 mb-4"
      v-b-toggle.collapse-4
      variant="primary"
      >Colors</b-button
    >
    <b-collapse id="collapse-4" class="mt-2">
      <label id="colors-title"> Colors :</label>
      <b-card>
        <p class="card-text">Option 1 : Customize your palette :</p>
        <div id="colorbox">
          <div class="d-flex p-3 bg-light text-white">
            <input id="ci" type="color" v-model="color" />
            <b-button @click="addcolor" variant="primary">Add color</b-button>
          </div>
          <b-form-tags
            input-id="tags-pills"
            v-model="colors"
            tag-variant="primary"
            tag-pills
            size="lg"
            separator=" "
            placeholder="Enter new colors separated by space"
          ></b-form-tags>
          <p class="mt-2">Current Palette: {{ colors }}</p>
          <b-button @click="customized" variant="primary"
            >Apply to chart</b-button
          >
        </div>
        <p class="card-text">Option 2 : Available palettes :</p>
        <b-button variant="secondary" v-b-toggle.collapse-1-inner size="sm"
          >Show
        </b-button>
        <b-collapse id="collapse-1-inner" class="mt-2">
          <button id="tt" @click="palette">Orange</button>
          <button id="dark" @click="palette2">Blue</button>
          <button id="green" @click="palette3">Green</button>
          <button id="light" @click="palette4">Light</button>
          <button id="rainbow" @click="rainbow">Rainbow</button>
        </b-collapse>
      </b-card>
    </b-collapse>
  </div>
</template>
<script>
import * as d3 from 'd3'
export default {
  props: ['inputData'],
  data () {
    return {
      colors: [],
      source: '',
      color: ''
    }
  },
  methods: {
    customized () {
      const vm = this
      d3.selectAll('path')
        .transition()
        .duration(1000)
        .style('fill', function (d, i) {
          if (i % 2 === 0) {
            return vm.colors[1]
          } else {
            return vm.colors[Math.floor(Math.random() * vm.colors.length)]
          }
        })
    },
    palette () {
      d3.selectAll('path')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return 'hsl(' + Math.random() * 50 + ',100%,50%)'
        })
    },

    addcolor () {
      this.colors.push(this.color)
    },
    palette2 () {
      d3.selectAll('path')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return (
            'hsl(' + 240 + ',' + 86 + '%' + ',' + Math.random() * 70 + '%' + ')'
          )
        })
    },
    palette3 () {
      d3.selectAll('path')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return (
            'hsl(' +
            121 +
            ',' +
            63 +
            '%' +
            ',' +
            Math.random() * 70 +
            80 +
            '%' +
            ')'
          )
        })
    },
    palette4 () {
      d3.selectAll('path')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return (
            'hsl(' +
            180 +
            ',' +
            65 +
            '%' +
            ',' +
            Math.random() * 70 +
            80 +
            '%' +
            ')'
          )
        })
    },
    rainbow () {
      d3.selectAll('path')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return 'hsl(' + Math.random() * 360 + ',100%,50%)'
        })
    }
  },
  mounted: function () {
    var width = 450
    var height = 450
    var margin = 40

    // The radius of the pieplot is half the width or half the height (smallest one). I subtract a bit of margin.
    var radius = Math.min(width, height) / 2 - margin

    // append the svg object to the div called 'my_dataviz'
    var svg = d3
      .select('#test')
      .append('svg')
      .attr('width', width)
      .attr('height', height)
      .append('g')
      .attr('transform', 'translate(' + width / 2 + ',' + height / 2 + ')')

    // set the color scale
    const valeurs = this.inputData.map((item) => item.value)
    const texte = this.inputData.map((item) => item.label)
    const couleurs = this.inputData.map((item) => item.color)
    var color = d3.scaleOrdinal().domain(Object.keys(valeurs)).range(couleurs)

    // Compute the position of each group on the pie:
    var pie = d3.pie().value(function (d) {
      return d[1]
    })

    var dataReady = pie(Object.entries(valeurs))

    // Build the pie chart: Basically, each part of the pie is a path that we build using the arc function.
    svg
      .selectAll('whatever')
      .data(dataReady)
      .enter()
      .append('path')
      .attr(
        'd',
        d3
          .arc()
          .innerRadius(100) // This is the size of the donut hole
          .outerRadius(radius)
      )
      .attr('fill', function (d) {
        return color(d.data[0])
      })
      .append('text')
      .text(texte)
      .attr('stroke', 'black')
      .style('stroke-width', '7px')
      .style('opacity', 5)
  }
}
</script>
<style scoped>
#rainbow {
  background-image: -webkit-gradient(
    linear,
    left top,
    right top,
    color-stop(0, #f22),
    color-stop(0.15, #f2f),
    color-stop(0.3, #22f),
    color-stop(0.45, #2ff),
    color-stop(0.6, #2f2),
    color-stop(0.75, #2f2),
    color-stop(0.9, #ff2),
    color-stop(1, #f22)
  );

  background-image: gradient(
    linear,
    left top,
    right top,
    color-stop(0, #f22),
    color-stop(0.15, #f2f),
    color-stop(0.3, #22f),
    color-stop(0.45, #2ff),
    color-stop(0.6, #2f2),
    color-stop(0.75, #2f2),
    color-stop(0.9, #ff2),
    color-stop(1, #f22)
  );
  border: none;
  width: 168px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
#tt {
  background-color: #f7b42c;
  background-image: linear-gradient(315deg, #f7b42c 0%, #fc575e 74%);

  border: none;
  width: 168px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
#light {
  background-color: #4dccc6;
  background-image: linear-gradient(315deg, #4dccc6 0%, #96e4df 74%);
  border: none;
  width: 168px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
#green {
  background-color: #f7b42c;
  background-color: #63d471;
  background-image: linear-gradient(315deg, #63d471 0%, #233329 74%);

  border: none;
  width: 168px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
#dark {
  background-color: #f7b42c;
  background-image: linear-gradient(147deg, #000000 0%, #04619f 74%);

  border: none;
  width: 168px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
</style>
