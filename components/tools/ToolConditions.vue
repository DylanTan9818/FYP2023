<template lang="pug">
div.d-grid
  v-row
    v-col(:cols="4")
      v-card.mx-auto.rounded-xl.pa-4(outlined)
        div Last checked value of
        h2 Temperature
        .data-container
          p {{ temperatureData }}
    v-col(:cols="4")
      v-card.mx-auto.rounded-xl.pa-4(outlined)
        div Last checked value of
        h2 Humidity
        div.text-primary +23% since yesterday
    v-col(:cols="4")
      v-card.mx-auto.rounded-xl.pa-4(outlined)
        div Last checked value of
        h2 Carbon Dioxide
        div.text-primary +36% since yesterday
    v-col(:cols="4")
      v-card.mx-auto.rounded-xl.pa-4(outlined)
        div Last checked value of
        h2 Carbon Monoxide
        div.text-primary +30% since yesterday
    v-col(:cols="4")
      v-card.mx-auto.rounded-xl.pa-4(outlined)
        div Last checked value of
        h2 TVOC
        div.text-primary +25% since yesterday
        v-col(:cols="4")
    v-card.mx-auto.rounded-xl.pa-4(outlined)
        div Last checked value of
        h2 PM2.5
        div.text-primary +25% since yesterday
        v-col(:cols="4")
</template>

<script>
import { mapGetters } from 'vuex'
import firebase from 'firebase/compat/app'
import 'firebase/compat/database'

export default {
  name: 'ToolConditions',
  data () {
    return {
      temperatureData: 0,
      coData: 0,
      ppmData: 0,
      humidityData: 0,
      pm25Data: 0
    }
  },
  computed: {
    ...mapGetters({
    })
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
      this.temperatureData = Object.values(data.temperature).slice(-1)
      this.timestamps = Object.values(data.timestamp).slice(-1)
      this.luxData = Object.values(data.tvoc).slice(-1)
      this.coData = Object.values(data.carbon_monoxide).slice(-1)
      this.humidityData = Object.values(data.humidity).slice(-1)
      this.ppmData = Object.values(data.co2).slice(-1)
      this.pm25Data = Object.values(data.pm25).slice(-1)
    })
  },
  methods: {
  }
}
</script>
