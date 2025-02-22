<template>
  <div class="expandHeader">
    <v-container>
      <!--
    =====================================================================================
        Layout Title
    =====================================================================================
    -->
      <the-layout-header
        :title="$t('home.create-wallet.title')"
        :subtitle-line-one="$t('home.create-wallet.subtitle-one')"
        :subtitle-line-two="$t('home.create-wallet.subtitle-two')"
        :route-obj="titleRoute"
        has-link
      />
      <!--
    =====================================================================================
        Options
    =====================================================================================
    -->
      <div style="max-width: 650px" class="mx-auto">
        <mew-button
          v-for="(btn, key) in buttons"
          :key="key"
          has-full-width
          class="mb-5 py-6"
          style="height: initial; min-height: 157px"
          :color-theme="btn.color"
          :btn-style="btn.style === 'outline' ? 'outline' : ''"
          @click.native="btn.fn"
        >
          <div v-if="btn.official" class="chip-official d-flex align-center">
            <v-icon size="15px" class="mr-1">mdi-shield-check</v-icon>
            <div
              class="font-weight-medium letter-spacing--initial line-height--initial"
            >
              Official
            </div>
          </div>
          <div
            v-if="!btn.recommended"
            class="orangePrimary--text mew-label note-position d-flex align-center"
          >
            <v-icon size="18px" class="mr-1">mdi-shield-alert</v-icon>
            NOT RECOMMENDED
          </div>
          <div class="width--full d-flex align-center text-left">
            <img
              v-if="btn.icon && !isMobile"
              class="ml-5 mr-6"
              :src="btn.icon"
              :alt="btn.alt"
              style="height: 70px"
            />
            <div class="px-3">
              <div class="d-flex align-center">
                <img
                  v-if="btn.icon && isMobile"
                  class="mr-4"
                  :src="btn.icon"
                  :alt="btn.alt"
                  style="height: 40px"
                />

                <div class="mew-heading-2 break-word letter-spacing--initial">
                  {{ btn.title }}
                </div>
              </div>
              <div
                class="mew-heading-4 reset-subtitle break-word letter-spacing--initial text-transform--none mt-4"
              >
                {{ btn.subtitle }}
              </div>
            </div>
          </div>
        </mew-button>
      </div>
    </v-container>
    <div class="spacer-y-medium" />
    <mew-toast
      ref="toast"
      can-close
      :link-obj="toastLink"
      text="Did you know? Hardware wallets offer the highest security for accessing your crypto."
      toast-type="info"
    />
    <!--
    =====================================================================================
      Create Wallet with Software Overlay - activates on Create Software Button click
    =====================================================================================
    -->
    <module-create-wallet-software
      :open="showSoftwareModule"
      :wallet-type="type"
      :close="closeSoftwareModule"
    />
  </div>
</template>

<script>
import ModuleCreateWalletSoftware from '@/modules/create-wallet/ModuleCreateWalletSoftware';
import { Toast, ERROR } from '@/modules/toast/handler/handlerToast';
import TheLayoutHeader from '../components-default/TheLayoutHeader';
import { ROUTES_HOME } from '@/core/configs/configRoutes';
import enkryptMarketing from '@/core/mixins/enkryptMarketing.mixin';

export default {
  name: 'TheCreateWalletLayout',
  components: {
    ModuleCreateWalletSoftware,
    TheLayoutHeader
  },
  mixins: [enkryptMarketing],
  props: {
    showSoftwareModule: {
      type: Boolean
    },
    type: {
      type: String,
      default: 'overview'
    }
  },
  data: () => ({
    toastLink: {
      title: 'Buy a hardware wallet',
      url: ''
    },
    titleRoute: {
      text: 'Access Wallet',
      routeName: 'AccessWallet'
    }
  }),
  computed: {
    buttons() {
      return [
        /* Enkrypt */
        {
          color: 'white',
          title: 'Install Enkrypt browser extension',
          subtitle:
            'MEW’s official browser extension. Connect to web3 on Ethereum and Polkadot, manage your NFTs, buy, send and swap',
          official: true,
          recommended: true,
          icon: require('@/assets/images/icons/icon-enkrypt-block.svg'),
          alt: 'Enkrypt',
          fn: () => {
            this.openEnkrypt();
          }
        },
        /* MEW wallet Button */
        {
          color: 'white',
          title: 'Download MEW wallet app',
          subtitle:
            'Our official mobile app to create your wallet, and connect to MEW Web using your mobile phone',
          official: true,
          recommended: true,
          icon: require('@/assets/images/icons/icon-mew-wallet.png'),
          alt: 'MEW wallet',
          fn: () => {
            this.openMewWallet();
          }
        },
        /* Hardware wallets */
        {
          color: 'white',
          title: 'Buy a hardware wallet',
          subtitle:
            'For the highest standard of security, buy a hardware wallet and use it with MEW',
          official: false,
          recommended: true,
          icon: require('@/assets/images/icons/icon-hardware-wallet.png'),
          alt: 'Hardware Wallets',
          fn: () => {
            this.$router.push({ name: ROUTES_HOME.BUY_HARDWARE_WALLET.NAME });
          }
        },
        /* Software */
        {
          color: 'white',
          style: 'outline',
          title: 'Software',
          subtitle:
            'Software methods like Keystore File and Mnemonic Phrase should only be used in offline settings by experienced users',
          official: false,
          recommended: false,
          fn: () => {
            this.openSoftwareModule();
          }
        }
      ];
    }
  },
  methods: {
    openSoftwareModule() {
      try {
        this.$router.push({
          name: ROUTES_HOME.CREATE_WALLET.NAME,
          params: { overlay: 'software' },
          query: { type: 'overview' }
        });
      } catch (e) {
        Toast(e, {}, ERROR);
      }
    },
    closeSoftwareModule() {
      try {
        this.$router.push({
          name: ROUTES_HOME.CREATE_WALLET.NAME
        });
      } catch (e) {
        Toast(e, {}, ERROR);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.reset-subtitle {
  line-height: 24px;
}

.note-position {
  position: absolute;
  top: 5px;
  right: 5px;
}

.chip-official {
  background-color: var(--v-greenPrimary-base);
  color: white;
  padding: 6px 10px;
  border-radius: 30px;
  @extend .note-position;
  top: -2px !important;
}
</style>
