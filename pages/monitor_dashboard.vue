<template lang="pug">
.tools-page.pa-0.ma-0.fill-width.dark-background
  v-row.ma-0.pt-16.px-8.fill-width
    v-row.px-7
      v-card.rounded-xl.px-0.mb-4(outlined).flex
        v-card-text.d-flex
          img(:src="require('../assets/upm.png')" :width="'400px'")
          .d-grid.ml-2.px-15
            h1.fw-600.secondary--text.mb-3 EMBEDDED SYSTEMS LAB
            p.font-weight-regular.subtitle-2 Building Health Index Rating Reference
            table.my-table
              thead
                tr
                  th.my-header-class Classification
                  th.my-header-class Score Range (%)
              tbody
                tr
                  td Excellent
                  td 80-100
                tr
                  td Good
                  td 70-79
                tr
                  td Moderate
                  td 50-69
                tr
                  td Poor
                  td 0-49

          ChatBot
    v-col(cols="9")
      line-chart
    v-col(cols="3")
      room-condition
</template>

<script>
import { mapGetters } from 'vuex'

import UpperTitle from '../components/UpperTitle.vue'

import RoomCondition from '~/components/room/RoomCondition.vue'
import LineChart from '~/components/room/LineChart.vue'
import ChatBot from '~/components/room/ChatBot.vue'

export default {
  name: 'ToolsPage',
  components: {
    UpperTitle,
    RoomCondition,
    LineChart,
    ChatBot
  },
  layout: 'default',
  data () {
    return {
    }
  },
  computed: {
    ...mapGetters({
    })
  },
  methods: {
  }
}
</script>

<style scoped>
:deep(.scroll) {
  overflow-x: hidden;
  overflow-y: auto;
  width: 100% !important;
}

.dark-background {
  background-color: #D2051E;
}

.fill-width {
  width: 100% !important;
}

/* Style the table */
.my-table {
  border-collapse: collapse;
  width: 100%;
  margin: 20px 0;
}

/* Style the table headers */
.my-table th {
  background-color: #D2051E;
  color: white;
  font-weight: bold;
  padding: 10px;
  text-align: left;
}

/* Style the table cells */
.my-table td {
  border: 1px solid #ddd;
  padding: 8px;
  font-weight: bold;
  text-align: left;
}

/* Style even rows with a background color */
.my-table tr:nth-child(even) {
  background-color: #f2f2f2;
}

/* Style table on hover */
.my-table tr:hover {
  background-color: #ddd;
}

</style>
