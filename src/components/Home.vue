<template>
  <div class="hello">
    <div class="container">
      <div class="row">
        <h2 class="title">Energy and Pitch</h2>
        <line-chart :chart-data="datacollection"></line-chart>
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
  import axios from 'axios'
  import LineChart from '@/components/LineChart'

  export default {
    name: 'home',
    components: {LineChart},
    data () {
      return {
        energy: null,
        pitch: null,
        phoneme_location: null,
        datacollection: null
      }
    },
    mounted () {
      this.fillData()
    },
    methods: {
      fillData () {
        axios.get('https://raw.githubusercontent.com/dazzyjong/dazzyjong.github.io/master/attention-prosody-chart.json')
          .then(response => {
            let results = response.data
            let phonemeLocationResult = results.phoneme_location
            let energyresult = results.energy
            let pitchresult = results.pitch

            console.log(phonemeLocationResult)
            console.log(energyresult)
            console.log(pitchresult)

            this.energy = energyresult
            this.pitch = pitchresult
            this.phoneme_location = phonemeLocationResult

            this.datacollection = {
              labels: this.phoneme_location,
              datasets: [
                {
                  label: 'Energy',
                  borderColor: '#f87979',
                  data: this.energy
                },
                {
                  label: 'Pitch',
                  borderColor: '#5bf8bf',
                  data: this.pitch
                }
              ]
            }
          })
          .catch(error => {
            console.log(error)
          })
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
    margin-top: 40px;
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
