<template>
  <div>
    <h2>Anuncis de licitació</h2>

    <div v-if="loading" class="placeholder">
      Carregant...
    </div>
    <div v-else-if="bids.length == 0" class="placeholder">
      No hi ha cap anunci de licitació obert
    </div>
    <div v-else>
      <open-bid v-for="bid in bids" :key="bid.id" :bid="bid" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import OpenBid from './OpenBid'

export default {
  name: 'open',
  components: {
    OpenBid
  },
  data () {
    return {
      loading: false,
      bids: []
    }
  },
  mounted () {
    this.loading = true
    this.setOpenBids()
  },
  methods: {
    setOpenBids () {
      axios.get('https://compromis.net/espai/contractors/bids/open')
        .then((response) => {
          this.bids = response.data.data
          this.loading = false
        })
        .catch(function (error) {
          console.log(error)
        })
    }
  }
}
</script>

<style lang="scss">
  @import '../variables';

  .placeholder {
    display: flex;
    align-items: center;
    justify-content: center;
    border: $default-border;
    color: $gray-text;
    font-size: 2rem;
    text-align: center;
    padding: 2rem;
    margin: 2rem 0;
    min-height: 124px;
  }
</style>
