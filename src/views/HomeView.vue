<template>
  <Hero> </Hero>
  <Features />

  <Cta>
    <template v-slot:col1>
      <div class="col cta-col cta-col-1 text-center">
        <span v-html="pageHomepage.data.attributes.CTA.cta_content"></span>
        <router-link class="oan-btn" to="/contact">{{ this.ctaBTN }}</router-link>
      </div>
    </template>

    <template v-if="pageHomepage.data.attributes.CTA.has_icon" v-slot:col2>
      <div class="col cta-col cta-col-2 d-flex">
        <img :src="pageHomepage.data.attributes.CTA.cta_icon.data.attributes.url" alt="" />
      </div>
    </template>
  </Cta>

  <Why-us />
  <Steps />
  <Testimonials />
  <Logos-slider />

  <Cta>
    <template v-slot:col1>
      <div class="col cta-col cta-col-1 text-center">
        <div v-html="pageHomepage.data.attributes.CTA_2.cta_content"></div>
        <router-link class="oan-btn" to="/contact">{{ this.ctaBTN }}</router-link>
      </div>
    </template>

    <template v-if="pageHomepage.data.attributes.CTA_2.has_icon" v-slot:col2>
      <div class="col cta-col cta-col-2 d-flex">
        <img :src="pageHomepage.data.attributes.CTA.cta_icon.data.attributes.url" alt="" />
      </div>
    </template>
  </Cta>

  <Footer />
</template>

<script>
import Hero from '@/components/Hero.vue'
import Features from '@/components/Features.vue'
import Cta from '@/components/Cta.vue'
import WhyUs from '@/components/Why-us.vue'
import Steps from '@/components/Steps.vue'
import Testimonials from '@/components/Testimonials.vue'
import LogosSlider from '@/components/Logos-slider.vue'
import Footer from '@/components/Footer.vue'
import gql from 'graphql-tag'

export default {
  components: {
    Footer,
    LogosSlider,
    Testimonials,
    Steps,
    WhyUs,
    Cta,
    Features,
    Hero
  },
  apollo: {
    pageHomepage: gql`
        query getHomepageData {
        pageHomepage(locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              CTA {
                has_icon
                cta_content
                cta_btn_txt
                cta_icon {
                  data {
                    attributes {
                      url
                      width
                    }
                  }
                }
              }
              CTA_2 {
                has_icon
                cta_content
                cta_btn_txt
                cta_icon {
                  data {
                    attributes {
                      url
                      width
                    }
                  }
                }
              }
              
            }
          }
        }
    }`
  },
  data() {
    return {
      pageHomepage: {
        data: {
          attributes: {
            CTA: {
              cta_content: '',
              cta_btn_txt: '',
              cta_icon: {
                data: {
                  attributes: {
                    url: '',
                    width: ''
                  }
                }
              }
            },
            CTA_2: {
              cta_content: '',
              cta_btn_txt: '',
              cta_icon: {
                data: {
                  attributes: {
                    url: '',
                    width: ''
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
    console.log('check build status 23.05.2022 ||')
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
