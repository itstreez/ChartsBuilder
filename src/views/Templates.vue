  /* eslint-disable */
<template>
  <html>
    <head>
      <meta name="viewport" content="width=device-width , initial-scale=1.0" />
    </head>
    <body>
      <nav>
        <div
          :class="['vertical-nav', 'bg-white', { active: sidebaropen }]"
          id="sidebar"
        >
          <div class="py-4 px-3 mb-4 bg-light">
            <div class="media d-flex align-items-center">
              <div class="media-body">
                <h4 class="m-0">Templates</h4>
                <p class="font-weight-normal text-muted mb-0">user</p>
              </div>
            </div>
          </div>
          <p
            class="text-gray font-weight-bold text-uppercase px-3 small pb-4 mb-0"
          >
            Tools
          </p>
          <ul class="nav flex-column bg-white mb-0">
            <li class="nav-item dropdown">
              <a href="#" class="nav-link text-dark">
                <i class="fa fa-adress-card mr-3 text-primary fa-fw"> </i>
                Type <span> &rsaquo; </span>
              </a>
              <ul>
                <li
                  v-for="(type, index) in chartTypes"
                  @click="selectType(index)"
                  :key="index"
                >
                  <a href="#"> {{ type.label }} </a>
                </li>
              </ul>
            </li>
            <li class="nav-item dropdown disabled">
              <a href="#" class="nav-link text-dark">
                <i class="fa fa-adress-card mr-3 text-primary fa-fw"> </i>
                Size <span> &rsaquo; </span>
              </a>
            </li>
            <li class="nav-item dropdown">
              <a href="#" class="nav-link text-dark">
                <i class="fa fa-adress-card mr-3 text-primary fa-fw"> </i>
                Current Data<span> &rsaquo; </span>
              </a>
              <ul>
                <table class="table table-dark">
                  <thead>
                    <tr>
                      <th>#</th>
                      <th>Label</th>
                      <th>Label2</th>
                      <th>Color</th>
                      <th>Value</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in inputData" :key="item.id">
                      <td>{{ item.id }}</td>
                      <td>{{ item.label }}</td>
                      <td>{{ item.label2 }}</td>
                      <td>{{ item.color }}</td>
                      <td>{{ item.value }}</td>
                    </tr>
                  </tbody>
                </table>
              </ul>
            </li>
          </ul>
        </div>
      </nav>
      <div class="page-content p-5" id="content">
        <button
          @click="sidebaropen = !sidebaropen"
          id="sidebarCollapse"
          type="button"
          class="btn btn-light bg-white rounded-pill shadow-sm px-4 mb-4"
        >
          <i class="fa fa-bars mr-2"></i
          ><small class="text-uppercase font-weight-bold">Toggle</small>
        </button>
        <b-button
          class="btn btn-light bg-white rounded-pill shadow-sm px-4 mb-4"
          v-b-toggle.collapse-1
          variant="primary"
        >
          Data</b-button
        >
        <b-collapse id="collapse-1" class="mt-2">
          <label id="data-title"> Data : </label>
          <b-card>
            <p class="card-text">Option 1 : Input your data :</p>
            <div id="colorbox">
              <div role="group">
                <label for="input-live">Label 1 :</label>
                <b-form-input
                  id="input-live"
                  v-model="valeur.label"
                  :state="label1State"
                  aria-describedby="input-live-help input-live-feedback"
                  placeholder="e.g : France"
                  trim
                ></b-form-input>
                <b-form-invalid-feedback id="input-live-feedback">
                  Can't be empty...
                </b-form-invalid-feedback>

                <label for="input-live">Label 2 :</label>
                <b-form-input
                  id="input-live"
                  v-model="valeur.label2"
                  :state="label2State"
                  aria-describedby="input-live-help input-live-feedback"
                  placeholder="e.g : 2005 "
                  trim
                ></b-form-input>
                <b-form-invalid-feedback id="input-live-feedback">
                  Must be a number for some types to work...
                </b-form-invalid-feedback>

                <label for="input-live">Color :</label>
                <input type="color" v-model="valeur.color" />
                <b-form-input
                  disabled
                  id="input-live"
                  v-model="valeur.color"
                  :state="colorState"
                  aria-describedby="input-live-help input-live-feedback"
                  placeholder="choose from color picker..."
                  trim
                ></b-form-input>

                <!-- This will only be shown if the preceding input has an invalid state -->
                <b-form-invalid-feedback id="input-live-feedback">
                  Can't be empty...
                </b-form-invalid-feedback>
                <label for="input-live">Value :</label>
                <b-form-input
                  id="input-live"
                  v-model="valeur.value"
                  :state="valueState"
                  aria-describedby="input-live-help input-live-feedback"
                  placeholder="e.g : 7820"
                  trim
                ></b-form-input>
                <b-form-invalid-feedback id="input-live-feedback">
                  Must be a number...
                </b-form-invalid-feedback>
                <b-button @click="addAll" variant="primary"
                  >Add Object</b-button
                >
                <b-button @click="empty" variant="danger"
                  >Delete Last Object</b-button
                >
              </div>
            </div>
            <p class="card-text">Option 2 : Data source :</p>
            <b-button variant="secondary" v-b-toggle.collapse-2-inner size="sm"
              >Show
            </b-button>
            <b-collapse id="collapse-2-inner" class="mt-2">
              <form id="form">
                <input type="file" id="inpFile" accept=".json" />
                <button type="submit">Upload File</button>
              </form>

              <b-button @click="fetchData" variant="primary">
                Fetch Data</b-button
              >
            </b-collapse>
          </b-card>
        </b-collapse>
        <h2 class="display-4 text-white">{{ title }}</h2>
        <div class="separator"></div>
        <div v-if="selectedType >= 0">
          <component
            :is="chartTypes[selectedType].component"
            :inputData="inputData"
            :key="chartTypes[selectedType].component + chartKey"
          >
          </component>
        </div>
      </div>
    </body>
  </html>
</template>
<script>
import axios from 'axios'
import Barchart from '../components/Barchart.vue'
import doghnut from '../components/doghnut.vue'
import lollilop from '../components/lollilop.vue'
import cube from '../components/cube.vue'
import circularbarplot from '../components/circularbarplot.vue'
import heatmap from '../components/heatmap.vue'
import linechart from '../components/linechart.vue'
import areachart from '../components/areachart.vue'
import scatter from '../components/scatter.vue'
// @ is an alias to /src

export default {
  components: {
    Barchart,
    doghnut,
    lollilop,
    cube,
    circularbarplot,
    heatmap,
    linechart,
    areachart,
    scatter
  },
  data () {
    return {
      chartKey: 0,
      source: '',
      inputData: [],
      valeur: { id: '', label: '', label2: '', color: '', value: '' },
      title: 'Pick your chart here :',
      selectedType: -1,
      chartTypes: [
        { label: 'Bar chart', component: 'Barchart', id: 1 },
        { label: 'doghnut', component: 'doghnut', id: 2 },
        { label: 'lollilop', component: 'lollilop', id: 3 },
        { label: 'cube', component: 'cube', id: 4 },
        { label: 'circular barplot', component: 'circularbarplot', id: 5 },
        { label: 'heatmap', component: 'heatmap', id: 6 },
        { label: 'linechart', component: 'linechart', id: 7 },
        { label: 'areachart', component: 'areachart', id: 8 },
        { label: 'scatter', component: 'scatter', id: 9 }
      ],
      sidebaropen: false
    }
  },
  computed: {
    label1State () {
      return this.valeur.label.length > 0
    },
    sourceState () {
      return this.source.length > 0
    },
    label2State () {
      return this.valeur.label2.length > 0
    },
    colorState () {
      return this.valeur.color.length > 0
    },
    valueState () {
      return !isNaN(this.valeur.value) && this.valeur.value.length > 0
    }
  },
  methods: {
    empty () {
      this.inputData.pop(this.valeur)
    },
    async fetchData () {
      try {
        const response = await axios.get('http://localhost:3000/objects')
        this.valeur = response.data
        this.inputData.push(this.valeur)
        this.valeur = { id: '', label: '', label2: '', color: '', value: '' }
        this.chartKey++
      } catch (e) {
        console.error(e)
      }
    },
    addAll () {
      this.valeur.id = Math.floor(Math.random() * 100)
      //  valeur: {id: '' , label: '' , label2:'' ,  color:'', value:'' },
      if (
        this.valeur.label === '' ||
        this.valeur.label2 === '' ||
        this.valeur.color === '' ||
        this.valeur.value === ''
      ) {
        alert('All Data fields are required')
      } else {
        this.inputData.push(JSON.parse(JSON.stringify(this.valeur)))
        this.valeur = { id: '', label: '', label2: '', color: '', value: '' }
        this.chartKey++
      }
    },
    selectType (index) {
      this.selectedType = index
      if (index === 0) {
        this.title = 'Bar chart :'
      } else if (index === 1) {
        this.title = 'Doghnut  :'
      } else if (index === 2) {
        this.title = 'Lollilop  :'
      } else if (index === 3) {
        this.title = 'Cube :'
      } else if (index === 4) {
        this.title = 'Circular Barplot  :'
      } else if (index === 5) {
        this.title = 'Heatmap  :'
      } else if (index === 6) {
        this.title = 'Linechart  :'
      } else if (index === 7) {
        this.title = 'Areachart  :'
      } else if (index === 8) {
        this.title = 'Scatter  :'
      }
    }
  },
  mounted: function () {
    const inpFile = document.getElementById('inpFile')
    const form = document.getElementById('form')
    form.addEventListener('submit', (e) => {
      e.preventDefault()
      const endpoint = 'http://localhost:3000/values'
      const formData = new FormData()
      console.log(inpFile.files)
      formData.append('inpFile', inpFile.files[0])
      fetch(endpoint, {
        method: 'post',
        body: formData
      }).catch(console.error)
    })
  }
}
</script>
<style scoped>
body {
  background: #599fd9;
  background-image: linear-gradient(
    135.9deg,
    rgba(109, 25, 252, 1) 16.4%,
    rgba(125, 31, 165, 1) 56.1%
  );
  min-height: 100vh;
  overflow-x: hidden;
}
#result {
  position: relative;
  left: 200px;
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
#bty {
  width: 250px;
}
#Rainbow {
  padding-right: 20px;
  border: 2px solid black;
  background-color: red;
  background: -moz-linear-gradient(
    right,
    orange,
    yellow,
    green,
    cyan,
    blue,
    violet
  );
  background: linear-gradient(
    to right,
    orange,
    yellow,
    green,
    cyan,
    blue,
    violet
  );
  -webkit-text-fill-color: transparent;

  border: none;
  width: 168px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
#colors-title {
  font-family: serif;
  font-size: 30px;
  color: white;
}
#data-title {
  font-family: serif;
  font-size: 30px;
  color: white;
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
#light {
  background-color: #f7b42c;
  background-color: #637081;
  background-image: linear-gradient(315deg, #637081 0%, #7c98b3 74%);

  border: none;
  width: 168px;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
.vertical-nav {
  min-width: 17rem;
  width: 17rem;
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  box-shadow: 3px 3px 10px rgba(0, 0, 0, 0.1);
  transition: all 0.4s;
}

.page-content {
  width: calc(100%-17rem);
  margin-left: 17rem;
  transition: all 0.4s;
}
#sidebar.active {
  margin-left: -17rem;
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
#content.active {
  width: 100%;
  margin: 0;
}
.separator {
  margin: 3rem 0;
  border-bottom: 1px dashed #fff;
}
.text-uppercase {
  letter-spacing: 0.1em;
}
.text-gray {
  color: #aaa;
}
.nav-link {
  text-transform: capitalize;
}
.nav-link:hover {
  background-image: linear-gradient(
    135.9deg,
    rgba(109, 25, 252, 1) 16.4%,
    rgba(125, 31, 165, 1) 56.1%
  );
}
.text-primary {
  color: #7579e7;
}
#ci {
  height: 50px;
  width: 300px;
}
@media (max-width: 768px) {
  #sidebar {
    margin-left: 0;
  }
  #content {
    width: 100%;
    margin: 0;
  }
  #content.active {
    margin-left: 17rem;
    width: calc();
  }
}
nav {
  position: absolute;
  z-index: 4;
}
nav ul ul {
  text-align: center;
  position: absolute;
  left: 250px;
  width: 360px;
  top: 0;
  display: none;
  background: whitesmoke;
  border-radius: 3px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 1);
}
nav ul span {
  position: absolute;
  right: 20px;
  font-size: 1.5em;
}
nav ul .dropdown {
  position: relative;
}
nav ul .dropdown:hover ul {
  display: initial;
}
nav ul li a {
  display: flex;
  align-items: center;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 1.15em;
  text-decoration: none;
  text-transform: capitalize;
  color: #000;
  padding: 30px 20px;
  height: 50px;
  transition: 0.5s ease;
  border-radius: 0 30px;
}
nav ul li a:hover {
  background-image: linear-gradient(
    135.9deg,
    rgba(109, 25, 252, 1) 16.4%,
    rgba(125, 31, 165, 1) 56.1%
  );
}
</style>
