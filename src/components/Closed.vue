<template>
  <div>
    <h2>Arxiu d'adjudicacions</h2>
    <vuetable ref="vuetable"
      api-url="https://compromis.net/espai/contractors/bids/closed"
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
  name: 'closed',
  components: {
    Vuetable,
    VuetablePagination
  },
  data () {
    return {
      fields: [
        {
          name: 'ref',
          sortField: 'ref',
          title: 'Núm.'
        },
        {
          name: 'title',
          sortField: 'title',
          title: 'Descripció'
        },
        {
          name: 'budget',
          sortField: 'budget',
          titleClass: 'text-right',
          dataClass: 'text-right',
          callback: 'formatNumber',
          title: 'Pressupost amb IVA'
        }
      ]
    }
  },

  methods: {
    formatNumber (value) {
      return accounting.formatMoney(value, '€', 2, '.', ',')
    }
  }
}
</script>

<style lang="scss">
.vuetable {
  font-family: Compromis, sans-serif;

  th {
    background: red;
    text-align: left;
    padding: 20px;
  }

  td {
    padding: 20px;
  }
}
</style>
