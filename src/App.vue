<template>
  <div id="app">
    <button @click="LoadHistoData">Refresh Slide Data</button>
    <h1>Hello Chart World.</h1>
    <bar-chart :chart-data="datacollection"></bar-chart>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import BarChart from './components/BarChart.js'
import axios from 'axios'

export default {
  name: 'app',
  components: {
    HelloWorld,
    BarChart
  },
data() {
return {
  arChartData: [],
  arChartLabels: [],
  strTest: 'hi',
  datacollection: null

}
},  
mounted () {
    this.LoadHistoData()
  },
methods: {
  LoadHistoData(){
    console.log('Hello LoadHistoData')
    console.log(this.strTest)
        this.arChartLabels = []
        this.arChartData = []

    // Need to assign this to an object so it can be referenced within Axios call
    let vue = this
      axios.post('http://localhost:2081/histodata', {
        firstName: 'Fred',
        lastName: 'Flintstone'
      })
    .then(function (response) {
      console.log(response)
      for(var i = 0; i < response.data.length; i++) {
        let strWhoPrinted = ''

        if (response.data[i].WhoPrinted === null) {
          strWhoPrinted = 'Unknown'
        } else {
          strWhoPrinted = response.data[i].WhoPrinted
        }  

        // Build Chart Data Array
        vue.arChartLabels.push(strWhoPrinted)
        vue.arChartData.push(response.data[i].TotalSlides)
     } // end for

        vue.datacollection = {
                  labels: vue.arChartLabels,
        datasets: [
          {
            label: 'Slides Cut',
            backgroundColor: '#f87979',
            data: vue.arChartData
          }
        ]

        }


      console.log('done test')
    })
    .catch(function (error) {
      console.log(error)
    })
    console.log('done')
  },
}
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
