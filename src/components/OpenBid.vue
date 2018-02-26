<template>
  <router-link :to="'/bid/' + bid.id" class="bid">
    <div class="bid-row">
      <div class="bid-ref"><span>{{ bid.ref }}</span></div>
      <div class="bid-title">{{ bid.title }}</div>
      <div class="bid-info">
        <div class="bid-budget">{{ bid.budget | formatMoney }}</div>
      </div>
    </div>
    <div class="bid-row bid-details">
      <div class="bid-published">{{ bid.published_on | formatDate }}</div>
      <div class="bid-entity">{{ bid.entity }}</div>
      <div v-if="deadlinePast" class="bid-deadline bid-review">En procés d'adjudicació</div>
      <div v-else class="bid-deadline">Data límit: <span>{{ bid.submission_deadline | formatDate }}</span></div>    </div>
  </router-link>
</template>

<script>
import accounting from 'accounting'
import dateFormat from 'dateformat'

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
      return dateFormat(new Date(value), 'dd/mm/yyyy')
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
  @import '../variables';

  .bid {
    display: block;
    font-family: Compromis, sans-serif;
    border: $default-border;
    margin: 2rem 0;
    padding: 2rem;
    transition: 0.2s;

    .bid-row {
      display: flex;
    }

    &:hover {
      background: $gray-100;

      .bid-ref span {
        background: #fff;
      }
    }

    &:active {
      background: $gray-200;
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
      color: $gray-text;
      background: $gray-100;
      padding: 5px 10px;
      border-radius: 5px;
      text-align: center;
    }
  }

  .bid-title {
    font-size: 1.75rem;
  }

  .bid-published {
    color: $gray-text;
    width: 100px;
  }

  .bid-entity {
    color: $gray-text;
  }

  .bid-details {
    margin-top: 1rem;
  }

  .bid-info {
    width: 200px;
    margin-left: auto;
    text-align: right;
  }

  .bid-budget {
    font-size: 2.5rem;
    color: $gray-text;
  }

  .bid-deadline {
    color: $gray-text;
    margin-left: auto;

    span {
      color: $success-color;
      font-weight: bold;
    }
  }

  .bid-review {
    color: $warning-color;
    font-weight: bold;
  }
</style>
