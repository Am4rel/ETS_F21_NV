<template>
  <div style="height: 100%">
    <v-container
      fluid
      style="position: relative; padding: 10px 30px;"
      v-if="currencies.length"
    >
      <div
        class="d-inline-flex justify-start flex-wrap"
        style="margin: auto"
      >
        <div
          class="card"
          v-for="currency in currencies"
          v-bind:key="currency.name"
        >
          <v-card>
            <v-card-title>
              {{ currency.name }}
            </v-card-title>
            <v-card-text>
              Amount: {{ currency.amount.toFixed(2) }}
            </v-card-text>
            <v-card-text>
              Updated at: {{ parseDate(new Date(currency.updatedAt)) }}
            </v-card-text>
            <v-card-actions class="actions d-flex">
              <v-btn
                text
              >
                See more / sell
              </v-btn>
            </v-card-actions>
          </v-card>
        </div>
      </div>
      <CurrencyPurchaseModal
        :userId="userId"
      />
      <v-btn
        class="buy-button"
        @click="setModal(true)"
      >
        Buy currency
      </v-btn>
    </v-container>
    <div
      v-else
      class="buy-suggestion"
    >
      <h2>You don't have any currencies yet</h2>
      <v-btn>Buy some</v-btn>
    </div>
  </div>
</template>

<script>
import { Component, Vue, namespace } from 'nuxt-property-decorator';
import CurrencyPurchaseModal from '~/components/CurrencyPurchaseModal.vue';

import headMixin from '~/helpers/mixins/headMixin'

const { State, Action } = namespace('userCurrencies');
const { Action: ModalAction } = namespace('purchaseModal');

@Component({
  components: {
    CurrencyPurchaseModal
  },
  mixins: [headMixin]
})

export default class TradePage extends Vue {
  @State currencies
  @Action fetchUserCurrencies

  @ModalAction setModal

  title = 'Trade'

  dialog = false
  userId = '61926bc6418dbb9a949cdeb1'
  fluid = true

  async mounted () {
    this.fetchUserCurrencies({
      userId: this.userId
    });
  }

  parseDate (date) {
    const day = date.getDate()
    const month = date.getMonth() + 1
    const year = date.getFullYear()
    return `${day}.${month}.${year}`
  }
  
}
</script>

<style>
.buy-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
}

.card {
  width: 270px;
  margin: 5px;
}

.card .actions {
  justify-content: center;
}

.buy-suggestion {
  margin-left: 20px;
}

</style>