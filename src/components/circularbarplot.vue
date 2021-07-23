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

    addcolor () {
      this.colors.push(this.color)
    },
    palette () {
      d3.selectAll('path')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return 'hsl(' + Math.random() * 50 + ',100%,50%)'
        })
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
    const margin = { top: 30, right: 30, bottom: 70, left: 60 }
    const width = 550 - margin.left - margin.right
    const height = 520 - margin.top - margin.bottom
    const innerRadius = 90
    const outerRadius = Math.min(width, height) / 2 // the outerRadius goes from the middle of the SVG area to the border

    // append the svg object
    const svg = d3
      .select('#test')
      .append('svg')
      .attr('width', width + margin.left + margin.right)
      .attr('height', height + margin.top + margin.bottom)
      .append('g')
      .attr(
        'transform',
        `translate(${width / 2 + margin.left}, ${height / 2 + margin.top})`
      )

    // Scales
    const x = d3
      .scaleBand()
      .range([0, 2 * Math.PI]) // X axis goes from 0 to 2pi = all around the circle. If I stop at 1Pi, it will be around a half circle
      .align(0) // This does nothing
      .domain(this.inputData.map((d) => d.label)) // The domain of the X axis is the list of states.
    const max = Math.max.apply(
      null,
      this.inputData.map((item) => item.value)
    )
    const y = d3
      .scaleRadial()
      .range([innerRadius, outerRadius]) // Domain will be define later.
      .domain([0, max]) // Domain of Y is from 0 to the max seen in the data
    // Add the bars
    svg
      .append('g')
      .selectAll('path')
      .data(this.inputData)
      .join('path')
      .attr('fill', (d) => d.color)
      .attr(
        'd',
        d3
          .arc() // imagine your doing a part of a donut plot
          .innerRadius(innerRadius)
          .outerRadius((d) => y(d.value))
          .startAngle((d) => x(d.label))
          .endAngle((d) => x(d.label) + x.bandwidth())
          .padAngle(0.01)
          .padRadius(innerRadius)
      )

    // Add the labels
    svg
      .append('g')
      .selectAll('g')
      .data(this.inputData)
      .join('g')
      .attr('text-anchor', function (d) {
        return (x(d.label) + x.bandwidth() / 2 + Math.PI) % (2 * Math.PI) <
          Math.PI
          ? 'end'
          : 'start'
      })
      .attr('transform', function (d) {
        return (
          'rotate(' +
          (((x(d.label) + x.bandwidth() / 2) * 180) / Math.PI - 90) +
          ')' +
          'translate(' +
          (y(d.value) + 10) +
          ',0)'
        )
      })
      .append('text')
      .text(function (d) {
        return d.label
      })
      .attr('transform', function (d) {
        return (x(d.label) + x.bandwidth() / 2 + Math.PI) % (2 * Math.PI) <
          Math.PI
          ? 'rotate(180)'
          : 'rotate(-360)'
      })
      .style('font-size', '15px')
      .attr('alignment-baseline', 'middle')
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
