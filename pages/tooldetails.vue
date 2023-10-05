<template lang="pug">
.tools-page.pa-0.ma-0.fill-width.dark-background
  v-row.ma-0.pt-16.px-8.fill-width
    v-row.px-7
      v-card.rounded-xl.px-0.mb-4(outlined).flex
        v-card-text.d-flex
          img(:src="require('C:/Users/DylanTan9818/FYP2023/assets/upm.png')" :width="'300px'")
          .d-grid.ml-2.px-15
            h1.fw-600.secondary--text.mb-3 EMBEDDED SYSTEMS LAB
            p.font-weight-regular.subtitle-2 Building Health Index Rating Reference
            img(:src="require('C:/Users/DylanTan9818/FYP2023/assets/table.png')" :width="'350px'" :height="'150px'").px-0
    v-col(cols="9")
      tool-details.fill-height(:product="productData")
    v-col(cols="9")
      //- tool-conditions
      line-chart
    v-col(cols="3")
      tool-repair
</template>

<script>
import { mapGetters } from 'vuex'

import UpperTitle from '../components/UpperTitle.vue'

import ToolChartCopy from '../components/tools/ToolChartCopy.vue'
import ToolConditions from '../components/tools/ToolConditions.vue'
// import ToolDetails from '../components/tools/ToolDetails.vue'
import ToolRepair from '~/components/tools/ToolRepair.vue'
import LineChart from '~/components/tools/LineChart.vue'
import LineChartCopy from '~/components/tools/LineChartCopy.vue'

export default {
  name: 'ToolsPage',
  components: {
    UpperTitle,
    ToolChartCopy,
    ToolConditions,
    ToolRepair,
    LineChart,
    LineChartCopy
  },
  layout: 'default',
  data () {
    return {
      // search: null
      productData: {
        name: 'SF 4-A22 CORDLESS DRILL DRIVER',
        id: 'T10-12118A',
        warranty: '2023-06-25',
        status: 'Normal',
        checktime: '12:57 PM, 11/5/2023',
        image: 'https://shorturl.at/MPRV6'
      }
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
</style>
