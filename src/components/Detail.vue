<template>
  <div class="bid">
    <router-link to="/" class="btn">Torna al llistat</router-link>
    <h2>{{ bid.title }}</h2>
    <div class="bid-ref">{{ bid.ref }}</div>

    <div class="bid-block">
      <h3>1. ENTITAT</h3>
      <div class="bid-block-content">
        {{ bid.entity }}
      </div>
    </div>

    <div class="bid-block">
      <h3>2. OBJECTE DEL CONTRACTE</h3>
      <table class="table">
        <tr>
          <th>Descripció</th>
          <td colspan="3">
            <nl2br v-if="bid.description" tag="div" :text="bid.description" />
            <em v-else>Sense especificar</em>
          </td>
        </tr>
        <tr>
          <th>Lloc de lliurament</th>
          <td>{{ bid.location }}</td>
          <th>Termini de lliurament</th>
          <td>{{ bid.deadline | formatDate }}</td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>3. TRAMITACIÓ I PROCEDIMENT D'ADJUDICACIÓ</h3>
      <table class="table">
        <tr>
          <th>Tramitació</th>
          <td>{{ processing }}</td>
          <th>Procediment</th>
          <td>{{ bid.procedure }} *</td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>4. PRESSUPOST</h3>
      <table class="table">
        <tr>
          <th>Import amb IVA</th>
          <td>{{ bid.budget | formatMoney }}</td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>5. REQUISITS ESPECÍFICS DEL CONTRACTISTA</h3>
      <div class="bid-block-text">
        <nl2br v-if="bid.requirements && bid.requirements != '0'" tag="div" :text="bid.requirements" />
        <em v-else>Sense especificar</em>
      </div>
    </div>

    <div class="bid-block">
      <h3>6. PRESENTACIÓ D'OFERTES</h3>
      <table class="table">
        <tr>
          <th>Documentació a presentar</th>
          <td colspan="3">
            <nl2br v-if="bid.submission_documents" tag="div" :text="bid.submission_documents" />
            <em v-else>Sense especificar</em>
          </td>
        </tr>
        <tr>
          <th>Lloc de presentació</th>
          <td>{{ bid.submission_location }}</td>
          <th>Data límit</th>
          <td>
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
            {{ bid.attachment }}
          </td>
        </tr>
      </table>
    </div>

    <div class="bid-block">
      <h3>7. ADJUDICACIÓ</h3>
      <table v-if="bid.awarded_to" class="table">
        <tr>
          <th>Adjudicatari</th>
          <td colspan="3">{{ bid.awarded_to }}</td>
        </tr>
        <tr>
          <th>Import adjudicat amb IVA</th>
          <td>{{ bid.awarded_amount | formatMoney }}</td>
          <th>Data</th>
          <td>{{ bid.awarded_date | formatDate }}</td>
        </tr>
      </table>
      <div v-else class="bid-block-text">
        <em>Per adjudicar</em>
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
</style>
