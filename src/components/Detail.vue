<template>
  <div class="bid">
    <router-link to="/" class="btn">Torna al llistat</router-link>
    <h2>{{ bid.title }}</h2>
    <div class="bid-ref">{{ bid.ref }}</div>

    <div class="bid-block">
      <h3>1. ENTITAT LICITADORA</h3>
      <div class="bid-block-content">
        {{ bid.entity }}
      </div>
    </div>

    <div class="bid-block">
      <h3>2. OBJECTE DEL CONTRACTE</h3>
      <table class="table">
        <tr>
          <th width="25%">Descripció</th>
          <td colspan="3">
            <nl2br v-if="bid.description" tag="div" :text="bid.description" />
            <em class="faded" v-else>Sense especificar</em>
          </td>
        </tr>
        <tr>
          <th>Lloc de lliurament</th>
          <td>{{ bid.location }}</td>
          <th width="25%">Termini de lliurament<br />del servei</th>
          <td width="25%">{{ bid.deadline | formatDate }}</td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>3. TRAMITACIÓ I PROCEDIMENT D'ADJUDICACIÓ</h3>
      <table class="table">
        <tr>
          <th width="25%">Tramitació</th>
          <td width="25%">{{ processing }}</td>
          <th width="25%">Procediment</th>
          <td width="25%">{{ bid.procedure }} *</td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>4. PRESSUPOST MÀXIM</h3>
      <table class="table">
        <tr>
          <th width="25%" style="vertical-align: middle">Import sense IVA</th>
          <td><span class="currency">{{ bid.budget | formatMoney }}</span></td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>5. CRITERIS DE VALORACIÓ DE LES OFERTES</h3>
      <table class="table">
        <tr>
          <th width="25%">Criteris</th>
          <td>
            <nl2br v-if="bid.requirements && bid.requirements != '0'" tag="div" :text="bid.requirements" />
            <em class="faded" v-else>Sense especificar</em>
          </td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>6. PRESENTACIÓ D'OFERTES</h3>
      <table class="table">
        <tr>
          <th width="25%">Documentació a presentar</th>
          <td colspan="3">
            <nl2br v-if="bid.submission_documents" tag="div" :text="bid.submission_documents" />
            <em class="faded" v-else>Sense especificar</em>
          </td>
        </tr>
        <tr>
          <th>Lloc de presentació</th>
          <td width="25%">{{ bid.submission_location }}</td>
          <th width="25%">Data límit</th>
          <td width="25%">
            <span>
              {{ bid.submission_deadline | formatDate }}
            </span>
          </td>
        </tr>
        <tr>
          <th>Termini a mantenir oferta</th>
          <td>{{ bid.submission_commitment }}</td>
          <th>Document adjunt</th>
          <td>
            <a v-if="bid.attachment" :href="bid.attachment" target="_blank" rel="noopener">{{ bid.attachment }}</a>
            <em class="faded" v-else>Cap</em>
          </td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>7. ADJUDICACIÓ</h3>
      <table v-if="bid.awarded_to" class="table">
        <tr>
          <th width="25%">Adjudicatari</th>
          <td colspan="3">{{ bid.awarded_to }}</td>
        </tr>
        <tr>
          <th>Import adjudicat amb IVA</th>
          <td width="25%"><span class="currency">{{ bid.awarded_amount | formatMoney }}</span></td>
          <th width="25%">Data</th>
          <td width="25%">{{ bid.awarded_date | formatDate }}</td>
        </tr>
      </table>
      <div v-else class="bid-block-text">
        <em class="faded">Per adjudicar</em>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import accounting from 'accounting'
import Nl2br from 'vue-nl2br'

export default {
  name: 'detail',
  components: {
    Nl2br
  },
  data () {
    return {
      bid: {}
    }
  },
  mounted () {
    this.setBid()
  },
  filters: {
    formatMoney: function (value) {
      return accounting.formatMoney(value, '€ ', 0, '.', ',')
    },
    formatDate: function (value) {
      return value
    }
  },
  computed: {
    processing: function () {
      return this.bid.processing
    }
  },
  methods: {
    setBid () {
      axios.get('https://compromis.net/espai/contractors/bid/' + this.$route.params.id)
        .then((response) => {
          this.bid = response.data
        })
        .catch((error) => {
          console.log(error)
        })
    }
  }
}
</script>

<style lang="scss">
@import '../variables';
  .bid {
    font-family: Compromis, sans-serif;
  }

  .bid-block {
    border: $default-border;
    margin-bottom: 2rem;
    border-bottom-width: 1px;

    h3 {
      background: $gray-100;
      font-size: 1.35rem;
      letter-spacing: 1px;
      padding: 0.75rem 1.5rem;
      border-bottom: 1px $gray-200 solid;
    }

    .table {
      th,
      td {
        padding: 1rem 1.5rem;
        background: #fff !important;
        border-bottom: 1px $gray-300 solid;
        vertical-align: top;
      }

      th {
        text-align: right;
      }
    }

    .bid-block-text,
    .bid-block-content {
      padding: 1rem 1.5rem;
      border-bottom: 1px $gray-300 solid;
    }

    .bid-block-content,
    .currency {
      font-size: 2rem;
    }

    .faded {
      color: $gray-text;
    }
  }

  @media only screen and (max-width: 760px) {
    .bid {
      table, thead, tbody, th, td, tr {
        display: block;
        width: 100%;
      }

      .table th {
        text-align: left;
        border-bottom: 0;
        padding-bottom: 0;
      }

      td {
        background: $gray-100;
      }
    }
  }
</style>
