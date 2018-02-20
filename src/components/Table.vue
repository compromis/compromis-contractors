<template>
  <div>
    <vuetable ref="vuetable"
      api-url="https://vuetable.ratiw.net/api/users"
      :fields="fields">
    </vuetable>
    <vuetable-pagination ref="pagination"></vuetable-pagination>
  </div>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'
import VuetablePagination from 'vuetable-2/src/components/VuetablePagination'

export default {
  name: 'HelloWorld',
  components: {
    Vuetable,
    VuetablePagination
  },
  data () {
    return {
      fields: ['name', 'email', 'birthdate']
    }
  }
}
</script>

<style scoped>

</style>
