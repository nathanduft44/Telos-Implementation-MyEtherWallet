<template>
  <mew-overlay
    :footer="{
      text: 'Need help?',
      linkTitle: 'Contact support',
      link: 'mailto:support@myetherwallet.com'
    }"
    :show-overlay="onRegister"
    :title="$t('ens.register-domain')"
    content-size="xlarge"
    :close="close"
  >
    <mew-stepper :items="stepperItems" :on-step="onStep">
      <template #stepperContent1
        ><request
          v-if="onStep === 1"
          class="mt-3"
          :name="name"
          :host-name="parsedHostName"
          :loading="checkingDomainAvail"
          :get-rent-price="getRentPrice"
          @onRequest="onRequest"
      /></template>
      <template #stepperContent2>
        <register
          v-if="onStep === 2"
          class="mt-3"
          :name="name"
          :duration="duration"
          :register="register"
          :not-enough-funds="notEnoughFunds"
          :no-funds-for-reg-fees="noFundsForRegFees"
          :commit-fee-in-eth="commitFeeInEth"
          :commit-fee-in-wei="commitFeeInWei"
          :commit-fee-usd="commitFeeUsd"
          :total-cost="totalCost"
          :total-cost-usd="totalCostUsd"
          :waiting-for-reg="waitingForReg"
          :commit="commit"
          :committed="committed"
          :minimum-age="minimumAge"
          :loading-commit="loadingCommit"
          :loading-reg="loadingReg"
      /></template>
      <template #stepperContent3><complete v-if="onStep === 3" /></template>
    </mew-stepper>
  </mew-overlay>
</template>

<script>
import Request from '../components/register/RegisterRequest';
import Register from '../components/register/Register';
import { ROUTES_WALLET } from '@/core/configs/configRoutes';
import BigNumber from 'bignumber.js';
import { mapGetters, mapState } from 'vuex';

export default {
  components: { Request, Register },
  props: {
    getRentPrice: {
      default: function () {
        return {};
      },
      type: Function
    },
    commit: {
      default: function () {
        return {};
      },
      type: Function
    },
    onRegister: { default: false, type: Boolean },
    close: {
      default: function () {
        return {};
      },
      type: Function
    },
    register: {
      default: function () {
        return {};
      },
      type: Function
    },
    generateKeyPhrase: {
      default: function () {
        return {};
      },
      type: Function
    },
    waitingForReg: {
      default: false,
      type: Boolean
    },
    notEnoughFunds: {
      default: false,
      type: Boolean
    },
    noFundsForRegFees: {
      default: false,
      type: Boolean
    },
    commitFeeInWei: {
      type: String,
      default: ''
    },
    commitFeeInEth: {
      type: String,
      default: ''
    },
    commitFeeUsd: {
      type: String,
      default: ''
    },
    totalCost: {
      type: String,
      default: ''
    },
    totalCostUsd: {
      type: String,
      default: ''
    },
    loadingCommit: {
      default: false,
      type: Boolean
    },
    loadingReg: {
      default: false,
      type: Boolean
    },
    committed: {
      default: false,
      type: Boolean
    },
    minimumAge: {
      default: '',
      type: String
    },
    name: {
      default: '',
      type: String
    },
    parsedHostName: {
      default: '',
      type: String
    },
    checkingDomainAvail: {
      default: false,
      type: Boolean
    }
  },
  data() {
    return {
      duration: 1,
      onStep: 1,
      stepperItems: [
        {
          step: 1,
          name: 'STEP 1. Select Duration'
        },
        {
          step: 2,
          name: 'STEP 2. Create Commitment'
        },
        {
          step: 3,
          name: 'STEP 3. Complete registration'
        }
      ]
    };
  },
  computed: {
    ...mapGetters('global', ['network', 'gasPrice', 'gasPriceByType']),
    ...mapGetters('external', ['fiatValue']),
    ...mapState('wallet', ['balance', 'address', 'web3']),
    ...mapState('global', ['gasPriceType'])
  },
  balanceToWei() {
    return this.web3.utils.toWei(BigNumber(this.balance).toString(), 'ether');
  },
  watch: {
    onStep(newStep) {
      if (newStep == 2) {
        this.$router.push({ name: ROUTES_WALLET.ENS_2.NAME });
      } else if (newStep == 3) {
        this.$router.push({ name: ROUTES_WALLET.ENS_3.NAME });
      } else {
        this.$router.push({ name: ROUTES_WALLET.ENS_MANAGER.NAME });
      }
    }
  },
  mounted() {
    if (this.onStep == 1) this.$router.push({ name: ROUTES_WALLET.ENS_1.NAME });
  },
  methods: {
    onRequest(val) {
      this.duration = val;
      this.generateKeyPhrase();
      this.$emit('getCommitFeeOnly');
      this.onStep += 1;
    }
  }
};
</script>
