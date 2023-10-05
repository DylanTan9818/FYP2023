<template lang="pug">
v-row.tool-table.justify-center
  v-card.pa-4.rounded-xl(outlined)
    v-card-title
      p.mb-0 UPM FSKTM Smart Intergrated Building Health Index
      v-spacer
    hr.mx-2

    //- Datatable
    v-data-table(
      :headers="headers"
      :items="desserts"
      :search="search"
    )
      template(v-slot:item.status="{ item }")
        v-chip(
          :color="getColor(item.status)"
          outlined
          pill
        )
          p.mb-0 {{ item.status }}
          v-icon.ml-2(v-if="item.status === 'Moderate'" small) mdi-wrench
          v-icon.ml-2(v-if="item.status === 'Excellent'" small) mdi-check-circle
          v-icon.ml-2(v-if="item.status === 'Good'" small) mdi-clock-time-eight

      template(v-slot:item.action="{ item }")
        v-btn(
          icon
          :color="$vuetify.theme.themes.light.primary"
          :to="'/tooldetails'"
        )
          v-icon mdi-magnify

      template(v-slot:body.prepend)
          tr
            td.py-4
              v-text-field(v-model="toolid" type="text" label="Room ID" hide-details="auto" dense outlined)
            td.py-4
              v-text-field(v-model="toolname" type="text" label="Room Name" hide-details="auto" dense outlined)
            td.py-4
              v-text-field(v-model="time" type="text" label="Date & Time" hide-details="auto" dense outlined)
            td.py-4
              v-select.select-category(:items="statusList" label="Select a health index" v-model="status" hide-details="auto" multiple chips dense outlined)
                template(v-slot:selection="{ item, index }")
                  v-chip(:color="getColor(item)" outlined)
                    span {{ item }}
            td.py-4.text-center
              v-icon.black--text mdi-magnify
</template>

<script>
export default {
  name: 'ToolTable',
  data () {
    return {
      search: '',
      toolid: '',
      toolname: '',
      time: '',
      status: '',
      statusList: ['Excellent', 'Moderate', 'Good'],
      headers: [
        {
          text: 'Room ID',
          align: 'start',
          sortable: false,
          value: 'id'
        },
        { text: 'Room Name', value: 'name' },
        { text: 'Date & Time', value: 'dt' },
        {
          text: 'Health Index',
          align: 'center',
          value: 'status',
          filter: (value) => {
            if (this.status.length === 0) { return true }
            return this.status.includes(value)
          }
        },
        { text: 'Action', value: 'action', sortable: false }
      ],
      desserts: [
        {
          id: 'T10-12118A',
          name: 'EMBEDDED SYSTEMS LAB',
          dt: '12:57 PM, 3/10/2023',
          status: 'Excellent',
          action: ''
        },
        {
          id: 'T10-12119A',
          name: 'DATABASE LAB',
          dt: '06:48 PM, 3/10/2023',
          status: 'Excellent',
          action: ''
        },
        {
          id: 'T10-13418A',
          name: 'BILIK KULIAH 1',
          dt: '06:48 PM, 3/10/2023',
          status: 'Good',
          action: ''
        },
        {
          id: 'T10-12328A',
          name: 'BILIK KULIAH 2',
          dt: '06:48 PM, 3/10/2023',
          status: 'Excellent',
          action: ''
        },
        {
          id: 'T10-32342A',
          name: 'OPERATING SYSTEMS',
          dt: '06:48 PM, 3/10/2023',
          status: 'Excellent',
          action: ''
        },
        {
          id: 'T12-12111A',
          name: 'MULTIMEDIA ROOM',
          dt: '06:48 PM, 3/10/2023',
          status: 'Good',
          action: ''
        },
        {
          id: 'T13-11238A',
          name: 'PEJABAT AM',
          dt: '06:48 PM, 3/10/2023',
          status: 'Moderate',
          action: ''
        }
        // {
        //   id: 'T20-75868A',
        //   name: 'HDE 500-A22 CORDLESS',
        //   dt: '06:48 PM, 24/1/2023',
        //   status: 'Normal',
        //   action: ''
        // },
        // {
        //   id: 'T22-12118B',
        //   name: 'TE 2-A22 CORDLESS ROTARY HAMMER',
        //   dt: '06:48 PM, 24/1/2023',
        //   status: 'Normal',
        //   action: ''
        // },
        // {
        //   id: 'T30-12118B',
        //   name: 'TE 2-A22 CORDLESS ROTARY HAMMER',
        //   dt: '06:48 PM, 24/1/2023',
        //   status: 'Checking',
        //   action: ''
        // },
        // {
        //   id: 'T10-12118A',
        //   name: 'SID 4-A22 CORDLESS IMPACT DRIVER',
        //   dt: '06:48 PM, 24/1/2023',
        //   status: 'Normal',
        //   action: ''
        // }
      ]
    }
  },
  computed: {

  },
  methods: {
    getColor (status) {
      if (status === 'Excellent') {
        return this.$vuetify.theme.themes.light.green
      } else if (status === 'Good') {
        return this.$vuetify.theme.themes.light.blue
      } else {
        return this.$vuetify.theme.themes.light.primary
      }
    }
  }
}
</script>

<style scoped>
.width-80 {
  width: 90% !important;
}

:deep(.select-category .v-chip .v-chip__content) {
  font-size: 12px !important;
}

:deep(.select-category .v-chip.v-size--default) {
  height: 20px;
}

</style>
