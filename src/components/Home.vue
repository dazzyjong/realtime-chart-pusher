<template>
  <div class="hello">
    <div class="container">
      <div class="row">
        <h2 class="title">Energy and Pitch</h2>
        <line-chart :chart-data="energydatacollection" :height="300"></line-chart>
        <line-chart :chart-data="pitchdatacollection" :height="300"></line-chart>
        <vue-file-input :detectPaste="false" @change="onFileInputChange">
            Open or Drag your files here
        </vue-file-input>
        <button class="btn btn-primary" v-on:click="download">Download</button>
      </div>
    </div>
  </div>
</template>

<script>
  import LineChart from '@/components/LineChart'

  export default {
    name: 'home',
    components: {LineChart},
    data () {
      return {
        results: null,
        energy: null,
        pitch: null,
        phonemeLocation: null,
        energydatacollection: null,
        pitchdatacollection: null
      }
    },
    mounted () {
    },
    methods: {
      fillData () {
        let phonemeLocationResult = this.results.phoneme_location
        let energyresult = this.results.energy
        let pitchresult = this.results.pitch

        this.energy = energyresult
        this.pitch = pitchresult
        this.phonemeLocation = phonemeLocationResult

        this.energydatacollection = {
          labels: this.phonemeLocation,
          datasets: [
            {
              label: 'Energy',
              borderColor: '#f87979',
              data: this.energy
            }
          ]
        }
        this.pitchdatacollection = {
          labels: this.phonemeLocation,
          datasets: [
            {
              label: 'Pitch',
              borderColor: '#5bf8bf',
              data: this.pitch
            }
          ]
        }
      },
      onFileInputChange (e) {
        const reader = new FileReader()
        reader.onload = e => {
          try {
            this.results = JSON.parse(e.target.result)
            this.fillData()
          } catch (ex) {
            alert('ex when trying to parse json = ' + ex)
          }
        }
        reader.readAsText(e.detail.files[0])
      },
      download () {
        let link = document.createElement('a')
        link.href = 'data:text/json;charset=utf-8,' + encodeURIComponent(JSON.stringify(this.results, null, 4))
        link.download = '.json'
        link.click()
      }
    }
  }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  div.vue-file-input {
    outline: 2px dashed grey;
    outline-offset: -10px;
    background: lightcyan;
    color: dimgray;
    padding: 10px 10px;
    min-height: 50px;
    position: relative;
    cursor: pointer;
  }
  .title {
    text-align: center;
    margin-top: 20px;
  }
  .subtitle {
    text-align: center;
  }
  .form {
    max-width: 600px;
    width: 100%;
    margin: 20px auto 0 auto;
  }
  .form h4 {
    text-align: center;
    margin-bottom: 30px;
  }
  .btn {
    margin-top: 20px;
  }
  h1, h2 {
  font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
