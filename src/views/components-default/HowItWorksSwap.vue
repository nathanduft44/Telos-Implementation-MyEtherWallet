<template>
  <div class="component-container position--relative pb-12 pb-lg-0">
    <img
      src="@/assets/images/backgrounds/bg-bubbles.png"
      class="d-none d-lg-block block-background-pattern"
    />
    <div class="py-6 py-md-12" />
    <v-container>
      <v-row>
        <v-col cols="12" lg="6">
          <v-img
            class="mb-8 block-image"
            src="@/assets/images/snippets/swap-page.png"
          />
        </v-col>
        <v-col cols="12" lg="6" class="text-content pl-lg-12">
          <app-block-title
            no-page-title
            :data="titleData"
            class="mb-12 pl-12"
          />
          <div class="mb-4 d-flex">
            <div class="mr-3">
              <v-img src="@/assets/images/currencies/icon-btc-white.svg" />
            </div>
            <div>
              <div class="mb-1 font-weight-bold subtitle-1 white--text">
                {{
                  $t('howItWorks.swap.info-one.title')
                    | lokalise('howItWorks.swap.info-one.title')
                }}
              </div>
              <p class="white--text">
                {{
                  $t('howItWorks.swap.info-one.text')
                    | lokalise('howItWorks.swap.info-one.text')
                }}
              </p>
            </div>
          </div>

          <div class="mb-4 d-flex">
            <div class="mr-3">
              <v-img src="@/assets/images/currencies//icon-eth-white.svg" />
            </div>
            <div>
              <div class="mb-1 font-weight-bold subtitle-1 white--text">
                {{
                  $t('howItWorks.swap.info-two.title')
                    | lokalise('howItWorks.swap.info-two.title')
                }}
              </div>
              <p class="white--text">
                {{
                  $t('howItWorks.swap.info-two.text')
                    | $t('howItWorks.swap.info-two.text')
                }}
              </p>
            </div>
          </div>

          <div class="d-flex">
            <div class="mr-3">
              <v-img src="@/assets/images/icons/icon-fiat-white.svg" />
            </div>
            <div>
              <div class="mb-1 font-weight-bold subtitle-1 white--text">
                {{
                  $t('howItWorks.swap.info-three.title')
                    | lokalise('howItWorks.swap.info-three.title')
                }}
              </div>
              <p class="white--text">
                {{
                  $t('howItWorks.swap.info-three.text')
                    | lokalise('howItWorks.swap.info-three.text')
                }}
              </p>
              <!-- <div class="d-flex align-center mt-10">
                <a
                  href="#"
                  class="text-decoration--underline white--text font-weight-medium"
                  >Watch tutorial</a
                >
              </div> -->
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import AppBlockTitle from '@/core/components/AppBlockTitle';

export default {
  components: { AppBlockTitle },
  data: vm => ({
    titleData: {
      textProps: 'white--text',
      toptitle: '',
      title: vm.$t('howItWorks.swap.title'),
      description: vm.$t('howItWorks.swap.description'),
      centered: false
    }
  })
};
</script>

<style lang="scss" scoped>
.component-container {
  background-image: linear-gradient(to right, #f09819, #f9723f);
}

.block-background-pattern {
  position: absolute;
  top: 80px;
  left: 80px;
  height: 85%;
}

.block-image {
  box-shadow: 0px 0px 20px #0000001c;
}
</style>
