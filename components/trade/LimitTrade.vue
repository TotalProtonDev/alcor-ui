<template lang="pug">
.row
  .col-lg-6
    .d-flex.mb-1
      small.text-success Buy {{ quote_token.symbol.name }}
      small.text-mutted.small.align-self-end.ml-auto.cursor-pointer(@click="onSetAmount(parseFloat(baseBalance))") {{ baseBalance }}
        i.el-icon-wallet.ml-1

    el-form(ref="form" :rules="rules")
      el-form-item
        el-input(type="number" min="0.00000001" step="0.00000001" v-model="price" clearable @change="fixPrice()" @input="priceChange()" size="medium" align="right")
          span(slot="prefix").mr-1 PRICE
          span(slot="suffix").mr-1 {{ base_token.symbol.name }}

      el-form-item
        el-input(type="number" v-model="amount" @input="amountChange(false, true)" @change="setPrecisions" clearable size="medium")
          span(slot="prefix").mr-1 AMOUNT
          span(slot="suffix").mr-1 {{ quote_token.symbol.name }}

      .px-3
        el-slider(:step="1" v-model="eosPercent" :marks="{0: '0%', 25: '25%', 50: '50%', 75: '75%', 100: '100%'}")

      el-form-item(prop="total" :inline-message="true").mt-4
        el-input(type="number" v-model="total" @input="totalChange(false, true)"  @change="setPrecisions" size="medium")
          span(slot="prefix").mr-1 TOTAL
          span(slot="suffix").mr-1 {{ base_token.symbol.name }}

      el-form-item.mt-1
        // TODO разработать компонент которой чекает залогинен ли
        el-button(size="small" type="success" @click="buy('limit')").w-100 Buy {{ quote_token.str }}

  .col-lg-6
    .d-flex.mb-1
      small.text-danger Sell {{ quote_token.symbol.name }}
      small.text-mutted.small.align-self-end.ml-auto.cursor-pointer(@click="onSetAmount(parseFloat(tokenBalance))") {{ tokenBalance }}
        i.el-icon-wallet.ml-1

    el-form(ref="form" :rules="rules")
      el-form-item
        el-input(type="number" min="0" step="0.0001" value="0" v-model="price" clearable @change="fixPrice()"  @input="priceChange()" size="medium")
          span(slot="prefix").mr-1 PRICE
          span(slot="suffix").mr-1.ml-2 {{ base_token.symbol.name }}

      el-form-item
        el-input(type="number" v-model="amount" @input="amountChange(false, true)" @change="setPrecisions" clearable size="medium")
          span(slot="prefix").mr-1 AMOUNT
          span(slot="suffix").mr-1 {{ quote_token.symbol.name }}

      .px-3
        el-slider(:step="1" v-model="tokenPercent" :marks="{0: '0%', 25: '25%', 50: '50%', 75: '75%', 100: '100%'}")

      el-form-item(prop="total" :inline-message="true").mt-4
        el-input(type="number" v-model="total" @input="totalChange(false, true)"  @change="setPrecisions" size="medium")
          span(slot="prefix").mr-1 TOTAL
          span(slot="suffix").mr-1 {{ base_token.symbol.name }}

      el-form-item.mt-1
        el-button(size="small" type="danger" @click="sell('limit')").w-100 Sell {{ quote_token.str }}
</template>

<script>
import { mapGetters, mapState } from 'vuex'
import { tradeMixin, tradeChangeEvents } from '~/mixins/trade'

export default {
  mixins: [tradeMixin, tradeChangeEvents],

  computed: {
    ...mapState(['network']),
    ...mapState('market', ['base_token', 'quote_token']),
    ...mapGetters('market', [
      'sorted_asks',
      'sorted_bids',
      'tokenBalance',
      'baseBalance'
    ]),
    ...mapGetters(['user'])
  }
}
</script>

<style scoped lang="scss">
.cursor-pointer {
  cursor: pointer;
}
</style>
