<template>
  <div class="bid">
    <router-link to="/" class="back"><span class="glyphicon glyphicon-chevron-left"></span> Torna al llistat</router-link>

    <hr />

    <div class="row">
      <div class="col-sm-3 pull-right">
        <div class="bid-ref">{{ bid.ref }}</div>
        <div class="bid-published">{{ bid.published_on | formatDate }}</div>
      </div>
      <div class="col-sm-9">
        <h1 class="bid-title">{{ bid.title }}</h1>
      </div>
    </div>

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
          <td>
            <span v-if="bid.budget > 0" class="currency">{{ bid.budget | formatMoney }}</span>
            <em class="faded" v-else>A determinar</em>
          </td>
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

    <div v-if="!bid.awarded_to || showSubmissionDetails" class="bid-block">
      <h3>6. PRESENTACIÓ D'OFERTES</h3>
      <div v-if="deadlinePast" class="warning">
        <span class="glyphicon glyphicon-warning-sign"></span> Ja no s'accepten ofertes al haver passat el termini de presentació
      </div>
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
            <a v-if="bid.attachment" :href="baseUrl + bid.attachment" target="_blank" rel="noopener"><span class="glyphicon glyphicon-cloud-download"></span> {{ bid.attachment }}</a>
            <em class="faded" v-else>Cap</em>
          </td>
        </tr>
      </table>
    </div>
    <div v-else class="bid-block">
      <h3 class="clickable" @click="toggleSubmissionDetails">6. PRESENTACIÓ D'OFERTES <em class="faded pull-right">TANCAT</em></h3>
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
        <em class="faded">Pendent d'adjudicació</em>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import accounting from 'accounting'
import dateFormat from 'dateformat'
import Nl2br from 'vue-nl2br'

export default {
  name: 'detail',
  components: {
    Nl2br
  },
  data () {
    return {
      bid: {},
      showSubmissionDetails: false,
      baseUrl: 'https://compromis.net/espai/uploads/contractors/'
    }
  },
  mounted () {
    this.setBid()
  },
  filters: {
    formatMoney: function (value) {
      return accounting.formatMoney(value, '€ ', 2, '.', ',')
    },
    formatDate: function (value) {
      return (value) ? dateFormat(new Date(value), 'dd/mm/yyyy') : '--'
    }
  },
  computed: {
    processing: function () {
      return (this.bid.processing === 'urgent') ? 'Urgència'
        : (this.bid.processing === 'emergency') ? 'Emergència'
          : 'Ordinària'
    },
    deadlinePast: function () {
      const deadline = new Date(this.bid.submission_deadline)
      const now = new Date()

      return deadline < now
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
    },
    toggleSubmissionDetails () {
      this.showSubmissionDetails = !this.showSubmissionDetails
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../variables';
  .bid {
    font-family: Compromis, sans-serif;
  }

  hr {
    margin: 2rem 0;
  }

  .bid-title {
    margin: 0 0 2rem;
    line-height: 1.25;
  }

  .bid-ref {
    font-size: 3rem;
    color: $gray-text;
    margin-top: 1rem;
    text-align: right;
  }

  .bid-published {
    color: $gray-text;
    text-align: right;
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
      margin-bottom: 0;
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

  a.back {
    display: inline-block;
    border: 1px $orange solid;
    padding: 0.75rem 1.5rem;
    border-radius: 20px;
    transition: 0.2s;
    margin-top: 1rem;

    &:hover {
      background: $gradient;
      color: #fff;
    }

    &:active {

    }
  }

  h3.clickable {
    cursor: pointer;
    border-bottom: 1px $gray-300 solid;
  }

  .warning {
    background-color: lighten($warning-color, 45%);
    color: $warning-color;
    padding: 1rem 1.5rem;
    border-bottom: 1px lighten($warning-color, 25%) solid;
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
