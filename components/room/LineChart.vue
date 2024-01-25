<template lang="pug">
v-card.pa-4.rounded-xl(outlined)
  v-card-text
    h2.fw-600.secondary--text.mb-4 Indoor Monitoring Parameters
    v-card.rounded-xl.mb-4(outlined)
      v-card-text
        .d-flex
          v-img.icon(:src="require('../../assets/1.png')" width="50")
          .d-grid.ml-2
            h3.fw-600.secondary--text Temperature (°C)
            p.font-weight-regular.subtitle-2 Today, 11/5/2023
        canvas(ref="chart" id="chart" height="80")

    v-card.rounded-xl.mb-4(outlined)
      v-card-text
          .d-flex
            v-img.icon(:src="require('../../assets/4.png')" width="50")
            .d-grid.ml-2
              h3.fw-600.secondary--text Humidity (%)
              p.font-weight-regular.subtitle-2 Today, 13/8/2023
          canvas(ref="humiditychart" id="humiditychart" height="80")

    v-card.rounded-xl.mb-4(outlined)
      v-card-text
        .d-flex
          v-img.icon(:src="require('../../assets/5.png')" width="50")
          .d-grid.ml-2
            h3.fw-600.secondary--text Carbon Dioxide (ppm)
            p.font-weight-regular.subtitle-2 Today, 13/8/2023
        canvas(ref="ppmchart" id="ppmchart" height="80")

    v-card.rounded-xl.mb-4(outlined)
      v-card-text
        .d-flex
          v-img.icon(:src="require('../../assets/carbon.png')" width="50")
          .d-grid.ml-2
            h3.fw-600.secondary--text Carbon Monoxide (ppm)
            p.font-weight-regular.subtitle-2 Today, 13/8/2023
        canvas(ref="cochart" id="cochart" height="80")

    v-card.rounded-xl.mb-4(outlined)
      v-card-text
          .d-flex
            v-img.icon(:src="require('../../assets/6.png')" width="50")
            .d-grid.ml-2
              h3.fw-600.secondary--text Dust Particles (pm2.5)
              p.font-weight-regular.subtitle-2 Today, 13/8/2023
          canvas(ref="pm25chart" id="pm25chart" height="80")

    v-card.rounded-xl.mb-4(outlined)
      v-card-text
        .d-flex
          v-img.icon(:src="require('../../assets/2.png')")
          .d-grid.ml-2
            h3.fw-600.secondary--text TVOC (ppb)
            p.font-weight-regular.subtitle-2 Today, 13/8/2023
        canvas(ref="luxchart" id="luxchart" height="80")

</template>

<script>
import firebase from 'firebase/compat/app'
import 'firebase/compat/database'
import Chart from 'chart.js/dist/Chart.js'

export default {
  data () {
    return {
      temperatureData: [],
      timestamps: [],
      coData: [],
      ppmData: [],
      humidityData: [],
      pm25Data: [],
      chart: null,
      luxchart: null,
      soundchart: null,
      cochart: null,
      ppmchart: null,
      humiditychart: null,
      pm25chart: null
    }
  },
  mounted () {
    // Initialize Firebase
    const firebaseConfig = {
      apiKey: 'AIzaSyCVidPdGite1-F4F7vTtrC5pjWcYelkRzI',
      authDomain: 'fyp2023-e3d45.firebaseapp.com',
      databaseURL: 'https://fyp2023-e3d45-default-rtdb.asia-southeast1.firebasedatabase.app',
      projectId: 'fyp2023-e3d45',
      storageBucket: 'fyp2023-e3d45.appspot.com',
      messagingSenderId: '252452635094',
      appId: '1:252452635094:web:2615f12de00e1dabaea477',
      measurementId: 'G-RM1N2Y0WXD'
    }
    firebase.initializeApp(firebaseConfig)

    // Get reference to the 'test' node in Firebase
    const rpmRef = firebase.database().ref('test')

    // Listen for changes in the temperature and timestamp data
    rpmRef.on('value', (snapshot) => {
      const data = snapshot.val()
      this.temperatureData = Object.values(data.temperature).slice(-10)
      this.timestamps = Object.values(data.timestamp).slice(-10)
      this.luxData = Object.values(data.tvoc).slice(-10)
      this.coData = Object.values(data.carbon_monoxide).slice(-10)
      this.humidityData = Object.values(data.humidity).slice(-10)
      this.ppmData = Object.values(data.co2).slice(-10)
      this.pm25Data = Object.values(data.pm25).slice(-10)

      // Update the chart with the new data
      if (this.chart) {
        this.chart.data.datasets[0].data = this.temperatureData
        this.chart.data.labels = this.timestamps
        this.chart.update()
      } else {
        this.createTempChart()
      }

      // Update the chart with the new data
      if (this.luxchart) {
        this.luxchart.data.datasets[0].data = this.luxData
        this.luxchart.data.labels = this.timestamps
        this.luxchart.update()
      } else {
        this.createLuxChart()
      }

      // Update the chart with the new data
      if (this.cochart) {
        this.cochart.data.datasets[0].data = this.coData
        this.cochart.data.labels = this.timestamps
        this.cochart.update()
      } else {
        this.createCoChart()
      }

      // Update the chart with the new data
      if (this.ppm) {
        this.ppm.data.datasets[0].data = this.ppmData
        this.ppm.data.labels = this.timestamps
        this.ppm.update()
      } else {
        this.createPpmChart()
      }

      // Update the chart with the new data
      if (this.humidity) {
        this.humidity.data.datasets[0].data = this.humidityData
        this.humidity.data.labels = this.timestamps
        this.humidity.update()
      } else {
        this.createHumidityChart()
      }

      // Update the chart with the new data
      if (this.pm25) {
        this.pm25.data.datasets[0].data = this.pm25Data
        this.pm25.data.labels = this.timestamps
        this.pm25.update()
      } else {
        this.createPm25Chart()
      }
    })
  },
  methods: {
    createTempChart () {
      const ctx = document.getElementById('chart').getContext('2d')

      const gradient = ctx.createLinearGradient(0, 0, 0, 300)
      gradient.addColorStop(0, 'rgba(253, 126, 20, 1)')
      gradient.addColorStop(1, 'rgba(253, 126, 20, 0)')

      this.chart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.timestamps,
          datasets: [
            {
              label: 'Temperature',
              data: this.temperatureData,
              borderColor: '#FD7E14',
              backgroundColor: gradient
            }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }],
            xAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      })
    },
    createLuxChart () {
      const ctx = document.getElementById('luxchart').getContext('2d')

      const gradient = ctx.createLinearGradient(0, 0, 0, 300)
      gradient.addColorStop(0, 'rgba(24, 144, 255, 1)')
      gradient.addColorStop(1, 'rgba(24, 144, 255, 0)')

      this.luxchart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.timestamps,
          datasets: [
            {
              label: 'TVOC',
              data: this.luxData,
              borderColor: '#1890FF',
              backgroundColor: gradient
            }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }],
            xAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      })
    },
    createCoChart () {
      const ctx = document.getElementById('cochart').getContext('2d')

      const gradient = ctx.createLinearGradient(0, 0, 0, 300)
      gradient.addColorStop(0, 'rgba(202, 209, 111, 1)')
      gradient.addColorStop(1, 'rgba(202, 209, 111, 0)')

      this.cochart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.timestamps,
          datasets: [
            {
              label: 'Carbon Monoxide',
              data: this.coData,
              borderColor: '#CAD16F',
              backgroundColor: gradient
            }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }],
            xAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      })
    },
    createPpmChart () {
      const ctx = document.getElementById('ppmchart').getContext('2d')

      const gradient = ctx.createLinearGradient(0, 0, 0, 300)
      gradient.addColorStop(0, 'rgba(205, 124, 255, 1)')
      gradient.addColorStop(1, 'rgba(205, 124, 255, 0)')

      this.ppmchart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.timestamps,
          datasets: [
            {
              label: 'Carbon Dioxide',
              data: this.ppmData,
              borderColor: '#CD7CFF',
              backgroundColor: gradient
            }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }],
            xAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      })
    },
    createHumidityChart () {
      const ctx = document.getElementById('humiditychart').getContext('2d')

      const gradient = ctx.createLinearGradient(0, 0, 0, 300)
      gradient.addColorStop(0, 'rgba(255, 140, 154, 1)')
      gradient.addColorStop(1, 'rgba(255, 140, 154, 0)')

      this.humiditychart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.timestamps,
          datasets: [
            {
              label: 'Humidity',
              data: this.humidityData,
              borderColor: '#FF8C9A',
              backgroundColor: gradient
            }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }],
            xAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      })
    },
    createPm25Chart () {
      const ctx = document.getElementById('pm25chart').getContext('2d')

      const gradient = ctx.createLinearGradient(0, 0, 0, 300)
      gradient.addColorStop(0, 'rgba(255, 209, 140, 1)')
      gradient.addColorStop(1, 'rgba(255, 209, 140, 0)')

      this.humiditychart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: this.timestamps,
          datasets: [
            {
              label: 'pm2.5',
              data: this.pm25Data,
              borderColor: '#FFD18C',
              backgroundColor: gradient
            }
          ]
        },
        options: {
          scales: {
            yAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }],
            xAxes: [{
              gridLines: {
                display: false
              },
              ticks: {
                beginAtZero: true
              }
            }]
          }
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.fw-600 {
  font-weight: 600 !important;
}

.icon {
  max-width: 50px;
}
</style>
