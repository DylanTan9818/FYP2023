<template lang="pug">
.tools-page.pa-0.ma-0.fill-width
  v-row.ma-0.pt-8.justify-center
    h1(style="color: white;").font-weight-bold.pb-4 UPM BUILDING HEALTH INDEX
  v-row.ma-0.pt-8.justify-center
    img(:src="require('../assets/upm.png')" :width="'300px'").rounded-xl
  v-row.ma-0.pt-8.fill-width
    v-col(cols="12")
      tool-table.pt-12.align-center.px-4.pb-4
</template>

<script>
import { mapGetters } from 'vuex'
import ToolTable from '../components/tools/ToolTable.vue'
// import UpperTitle from '../components/UpperTitle.vue'

export default {
  name: 'ToolsPage',
  components: {
    // UpperTitle,
    ToolTable
    // ToolChartCopy,
    // ToolConditions,
    // ToolDetails,
    // ToolRepair
  },
  layout: 'default',
  data () {
    return {
      // search: null
      productData: {
        name: 'EMBEDDED SYSTEMS',
        id: '1234',
        warranty: '2023-06-25',
        status: 'Normal',
        checktime: '6:00 PM, 4/1/2023',
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

.fill-height {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}
</style>
