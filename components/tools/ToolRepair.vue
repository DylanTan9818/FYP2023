<template lang="pug">
  v-card.mx-auto.pa-2.rounded-xl(outlined)
    v-card-text
      h2.fw-600.secondary--text.pb-2 Indoor Room Health Index Classification
      ApexCharts.d-flex.justify-space-around.pt-2(type="donut" :options="chartOptions" :series="series" width="250" height="250")

    v-card-text.pt-0(class="text-center")
      h2.fw-600.secondary--text.mb-2 Health Index
      h2.primary--text.mb-4 Excellent

      h2.fw-600.secondary--text.mb-2 Date
      h3.secondary--text {{currentDate}}

      v-card.rounded-lg.mt-6.text-left(outlined)
        v-card-text
          v-img.icon(:src="require('../../assets/1.png')" width="70").mb-1
          h2.secondary--text.fw-600.mb-5 Temperature
          .data-container
          h1.fw-600.primary--text {{ temperatureData[0]  }} °C

      v-card.rounded-lg.mt-6.text-left(outlined)
        v-card-text
          v-img.icon(:src="require('../../assets/4.png')" width="70").mb-1
          h2.secondary--text.fw-600.mb-5 Humidity
          .data-container
          h1.fw-600.primary--text {{ humidityData[0]  }} %

      v-card.rounded-lg.mt-6.text-left(outlined)
        v-card-text
          v-img.icon(:src="require('../../assets/5.png')" width="70").mb-1
          h2.secondary--text.fw-600.mb-5 Carbon Dioxide
          .data-container
          h1.fw-600.primary--text {{ ppmData[0]  }} ppm

      v-card.rounded-lg.mt-6.text-left(outlined)
        v-card-text
          v-img.icon(:src="require('../../assets/carbon.png')" width="70").mb-1
          h2.secondary--text.fw-600.mb-5 Carbon Monoxide
          .data-container
          h1.fw-600.primary--text {{ coData[0] }} ppm

      v-card.rounded-lg.mt-6.text-left(outlined)
        v-card-text
          v-img.icon(:src="require('../../assets/6.png')" width="70").mb-1
          h2.secondary--text.fw-600.mb-5 PM2.5
          .data-container
          h1.fw-600.primary--text {{ pm25Data[0]}} µg/m³

      v-card.rounded-lg.mt-6.text-left(outlined)
        v-card-text
          v-img.icon(:src="require('../../assets/2.png')" width="70").mb-1
          h2.secondary--text.fw-600.mb-5 TVOC
          .data-container
          h1.fw-600.primary--text {{ tvocData[0] }} ppm

</template>

<script>
import firebase from 'firebase/compat/app'
import 'firebase/compat/database'
export default {
  name: 'ToolRepair',
  data () {
    return {
      currentDate: null,
      temperatureData: 0,
      coData: 0,
      ppmData: 0,
      humidityData: 0,
      tvocData: 0,
      pm25Data: 0,
      series: [92.0, 8.0],
      chartOptions: {
        chart: {
          type: 'donut'
        },
        legend: {
          show: false
        },
        labels: ['Prediction Accuracy', 'Prediction Accuracy'],
        colors: ['#D2051E', '#F9F4F4'],
        plotOptions: {
          pie: {
            expandOnClick: true,
            donut: {
              labels: {
                show: true,
                total: {
                  label: 'Accuracy',
                  showAlways: true,
                  show: true,
                  color: '#333333',
                  fontSize: '20px',
                  fontWeight: '600',
                  formatter: function (value) {
                    const t = 92.0
                    return t + '%'
                  }
                },
                value: {
                  color: '#D2051E',
                  fontSize: '28px',
                  fontWeight: '600'
                }
              }
            }
          }
        },
        responsive: [{
          breakpoint: 480,
          options: {
            chart: {
              width: 300
            },
            legend: {
              position: 'bottom'
            }
          }
        }],
        dataLabels: {
          enabled: false
        }
      }
    }
  },
  mounted () {
    // Initialize Firebase
    this.getCurrentDate()
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
      this.temperatureData = Object.values(data.temperature).slice(-1)
      this.timestamps = Object.values(data.timestamp).slice(-1)
      this.tvocData = Object.values(data.tvoc).slice(-1)
      this.coData = Object.values(data.carbon_monoxide).slice(-1)
      this.humidityData = Object.values(data.humidity).slice(-1)
      this.ppmData = Object.values(data.co2).slice(-1)
      this.pm25Data = Object.values(data.pm25).slice(-1)
    })
  },
  methods: {
    getCurrentDate () {
      const today = new Date()
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      this.currentDate = today.toLocaleDateString(undefined, options)
    }
  }
}
</script>

<style scoped>
.fw-600 {
  font-weight: 600 !important;
}
</style>
