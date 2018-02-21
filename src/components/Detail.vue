<template>
  <div>
    {{ bid }}
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'detail',
  components: {
  },
  data () {
    return {
      bid: {}
    }
  },
  mounted () {
    this.setBid()
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
