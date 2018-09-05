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
        
      </div>
    </div>
    <!-- <div class="container">
      <div class="row">
        <form class="form" @submit.prevent="addExpenses">
          <h4>Add New Entry</h4>
          <div class="form-group">
            <label>Expenses</label>
            <input class="form-control" placeholder="How much did you spend?" type="number" v-model="expenseamount" required>
          </div>
          <div class="form-group">
            <label>pitch</label>
            <input class="form-control" placeholder="How much did you earn?" type="number" v-model="peakamount" required>
          </div>
          <div class="form-group">
            <label>Date</label>
            <input class="form-control" placeholder="Date" type="phoneme_location" v-model="entrydate" required>
          </div>
          <div class="form-group">
            <button class="btn btn-primary">Add New Entry</button>
          </div>
        </form>
      </div>
    </div> -->
  </div>
</template>

<script>
  import LineChart from '@/components/LineChart'

  export default {
    name: 'home',
    components: {LineChart},
    data () {
      return {
        energy: null,
        pitch: null,
        phoneme_location: null,
        energydatacollection: null,
        pitchdatacollection: null
      }
    },
    mounted () {
    },
    methods: {
      fillData (results) {
        let phonemeLocationResult = results.phoneme_location
        let energyresult = results.energy
        let pitchresult = results.pitch

        this.energy = energyresult
        this.pitch = pitchresult
        this.phoneme_location = phonemeLocationResult

        this.energydatacollection = {
          labels: this.phoneme_location,
          datasets: [
            {
              label: 'Energy',
              borderColor: '#f87979',
              data: this.energy
            }
          ]
        }
        this.pitchdatacollection = {
          labels: this.phoneme_location,
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
            var json = JSON.parse(e.target.result)
            this.fillData(json)
          } catch (ex) {
            alert('ex when trying to parse json = ' + ex)
          }
        }
        reader.readAsText(e.detail.files[0])
      }
      // addExpenses () {

      // }
    }
  }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

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
