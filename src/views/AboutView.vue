<template>
  <AboutUsHero />
  <CompanyTimeline />
  <Counters />

  <Cta>
    <template v-slot:col1>
      <div class="col cta-col cta-col-1 text-center">
        <div v-html="pageAbout.data.attributes.CTA.cta_content"></div>
        <router-link class="oan-btn" to="/contact">{{ this.ctaBTN }}</router-link>
      </div>
    </template>

    <template v-if="pageAbout.data.attributes.CTA.has_icon" v-slot:col2>
      <div class="col cta-col cta-col-2 d-flex">
        <img :src="pageAbout.data.attributes.CTA.cta_icon.data.attributes.url" alt="" />
      </div>
    </template>
  </Cta>

  <Footer />
</template>

<script>
import gql from 'graphql-tag'
import Footer from '@/components/Footer.vue'
import AboutUsHero from '@/components/AboutUsHero.vue'
import CompanyTimeline from '@/components/CompanyTimeline.vue'
import Counters from '@/components/Counters.vue'
import Cta from '@/components/Cta.vue'

export default {
  components: {
    Cta,
    Counters,
    CompanyTimeline,
    AboutUsHero,
    Footer
  },
  apollo: {
    pageAbout: gql`
      query {
        pageAbout(locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              CTA {
                cta_content
                cta_btn_txt
                has_icon
                cta_icon {
                  data {
                    attributes {
                      url
                    }
                  }
                }
              }
            }
          }
        }
      }
    `
  },
  data() {
    return {
      pageAbout: {
        data: {
          attributes: {
            CTA: {
              cta_content: '',
              cta_btn_txt: '',
              has_icon: null,
              cta_icon: {
                data: {
                  attributes: {
                    url: ''
                  }
                }
              }
            }
          }
        }
      }
    }
  },
  created() {
    document.title = 'Online Advertising Network'
  },
  computed: {
    lang() {
      return localStorage.getItem('lang')
    },
    placeholder() {
      return this.lang == 'pl' ? 'Adres email' : 'Your email'
    },
    ctaBTN() {
      return this.lang == 'pl' ? 'Napisz do nas' : 'Write to us '
    }
  }
}
</script>
