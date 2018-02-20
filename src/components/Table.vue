<template>
  <div>
    <vuetable ref="vuetable"
      api-url="https://compromis.net/espai/contractors/bids"
      :fields="fields"
      pagination-path="">
    </vuetable>
    <vuetable-pagination ref="pagination"></vuetable-pagination>
  </div>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
import accounting from 'accounting'

export default {
  name: 'HelloWorld',
  components: {
    Vuetable,
    VuetablePagination
  },
  data () {
    return {
      fields: [
        {
          name: 'ref',
          sortField: 'ref'
        },
        {
          name: 'title',
          sortField: 'title',
        },
        {
          name: 'budget',
          sortField: 'budget',
          titleClass: 'text-right',
          dataClass: 'text-right',
          callback: 'formatNumber'
        }
      ]
    }
  },

  methods: {
    formatNumber (value) {
      return accounting.formatNumber(value, 2)
    }
  }
}
</script>

<style scoped>

</style>
