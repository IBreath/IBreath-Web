<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <line-chart :width="1000"
                    :height="200"
                    :chart-data="datacollection">

        </line-chart>
        <button @click="fillData()">Refresh</button>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
    import LineChart from './LineChart'
    import axios from 'axios';

    export default {
        components: {
            LineChart
        },
        data () {
            return {
                datacollection: null,
                measures : [],
                dates: [],
                data : [],
            }
        },
        created () {
            axios.get(`/api/measures/10/1`, {})
            .then(response => {
                this.data = response.data;
                this.measures = response.data.map(a => a.value);
                this.dates = response.data.map(a => { return a.dateTime[0] + '/' + a.dateTime[1] + '/' + a.dateTime[2];});
                this.fillData();
            })
            .catch(e => {
                this.errors.push(e)
            })
        },
        mounted () {
            this.fillData()
        },
        methods: {
            fillData () {
                this.datacollection = {
                    labels: this.dates,
                    datasets: [
                        {
                            label: 'Mesures',
                            backgroundColor: '#f87979',
                            data: this.measures,
                        }
                    ]
                }
            },
        }
    }
</script>

<style scoped>
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
