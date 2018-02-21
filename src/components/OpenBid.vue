<template>
  <router-link :to="'/bid/' + bid.id" class="bid">
    <div class="bid-ref"><span>{{ bid.ref }}</span></div>
    <div class="bid-title">{{ bid.title }}</div>
    <div class="bid-info">
      <div class="bid-budget">{{ bid.budget | formatMoney }}</div>
      <div v-if="deadlinePast" class="bid-deadline bid-review">En revisió</div>
      <div v-else class="bid-deadline">Data límit: <span>{{ bid.submission_deadline | formatDate }}</span></div>
    </div>
  </router-link>
</template>

<script>
import accounting from 'accounting'

export default {
  name: 'open-bid',
  props: {
    bid: Object
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
    deadlinePast: function () {
      const deadline = new Date(this.bid.submission_deadline)
      const now = new Date()

      return deadline < now
    }
  }
}
</script>

<style lang="scss" scoped>
  .bid {
    font-family: Compromis, sans-serif;
    display: flex;
    border: 2px #d1d2d3 solid;
    margin: 2rem 0;
    padding: 2rem;
    transition: 0.2s;

    &:hover {
      background: #f1f2f3;

      .bid-ref span {
        background: #fff;
      }
    }

    &:active {
      background: #e1e2e3;
    }
  }

  .bid-ref {
    flex-grow: 0;
    flex-shrink: 0;
    width: 100px;
    font-size: 1.75rem;
    padding-right: 2rem;

    span {
      display: block;
      color: #818283;
      background: #f1f2f3;
      padding: 5px 10px;
      border-radius: 5px;
      text-align: center;
    }
  }

  .bid-title {
    font-size: 1.75rem;
  }

  .bid-info {
    width: 200px;
    margin-left: auto;
    text-align: right;
  }

  .bid-budget {
    font-size: 2.5rem;
    color: #818283;
  }

  .bid-deadline {
    color: #818283;

    span {
      color: #2ecc71;
      font-weight: bold;
    }
  }

  .bid-review {
    color: #e67e22;
    font-weight: bold;
  }
</style>
