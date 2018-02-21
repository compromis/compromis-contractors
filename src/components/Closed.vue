<template>
  <div>
    <h2>Arxiu d'adjudicacions</h2>
    <vuetable ref="vuetable"
      api-url="https://compromis.net/espai/contractors/bids/closed"
      :fields="fields"
      pagination-path=""
      @vuetable:cell-clicked="goToDetail">
    </vuetable>
    <vuetable-pagination ref="pagination"></vuetable-pagination>
  </div>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'
import accounting from 'accounting'
import dateFormat from 'dateformat'

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
          title: 'Núm.'
        },
        {
          name: 'title',
          title: 'Descripció'
        },
        {
          name: 'awarded_amount',
          titleClass: 'text-right',
          dataClass: 'text-right',
          callback: 'formatNumber',
          title: 'Pressupost amb IVA'
        },
        {
          name: 'awarded_date',
          titleClass: 'text-right',
          dataClass: 'text-right',
          callback: 'formatDate',
          title: 'Adjudicat'
        }
      ]
    }
  },

  methods: {
    goToDetail (data) {
      this.$router.push('/bid/' + data.id)
    },
    formatNumber (value) {
      return accounting.formatMoney(value, '€', 2, '.', ',')
    },
    formatDate (value) {
      return dateFormat(new Date(value), 'dd/mm/yyyy')
    }
  }
}
</script>

<style lang="scss">
  @import '../variables';

  .vuetable {
    font-family: Compromis, sans-serif;
    margin-top: 2rem;

    th {
      text-align: left;
      padding: 1rem;
      background: $gray-200;
    }

    td {
      padding: 1rem;
      cursor: pointer;
    }

    tr:hover td {
      background: $gray-100;
      color: $orange;
    }

    tr:active td {
      background: darken($gray-100, 5%);
      color: darken($orange, 5%);
    }
  }

  .text-right {
    text-align: right !important;
  }
</style>
