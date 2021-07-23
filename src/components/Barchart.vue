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
          <button id="orange" @click="palette">Orange</button>
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
      d3.selectAll('rect')
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
      d3.selectAll('rect')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return 'hsl(' + Math.random() * 50 + ',100%,50%)'
        })
    },
    palette2 () {
      d3.selectAll('rect')
        .transition()
        .duration(1000)
        .style('fill', function () {
          return (
            'hsl(' + 240 + ',' + 86 + '%' + ',' + Math.random() * 70 + '%' + ')'
          )
        })
    },
    palette3 () {
      d3.selectAll('rect')
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
      d3.selectAll('rect')
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
      d3.selectAll('rect')
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

    // append the svg object to the body of the page
    const svg = d3
      .select('#test')
      .append('svg')
      .attr('width', width + margin.left + margin.right)
      .attr('height', height + margin.top + margin.bottom)
      .append('g')
      .attr('transform', `translate(${margin.left},${margin.top})`)
    // Parse the Data
    // X axis
    const x = d3
      .scaleBand()
      .range([0, width])
      .domain(this.inputData.map((d) => d.label))
      .padding(0.2)
    svg
      .append('g')
      .attr('transform', `translate(0, ${height})`)
      .call(d3.axisBottom(x))
      .selectAll('text')
      .attr('transform', 'translate(-10,0)rotate(-45)')
      .style('text-anchor', 'end')
      .style('font-size', 15)

    const max = Math.max.apply(
      null,
      this.inputData.map((item) => item.value)
    )
    const y = d3.scaleLinear().domain([0, max]).range([height, 0])
    svg
      .append('g')
      .call(d3.axisLeft(y))
      .selectAll('text')
      .style('font-size', 15)

    // Bars
    svg
      .selectAll('mybar')
      .data(this.inputData)
      .join('rect')
      .attr('x', (d) => x(d.label))
      .attr('y', (d) => y(d.value))
      .attr('width', x.bandwidth())
      .attr('height', (d) => height - y(d.value))
      .attr('fill', (d) => d.color)
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
#orange {
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
