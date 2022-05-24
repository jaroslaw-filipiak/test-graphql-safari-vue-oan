<template>
  <GDPRhero />

  <Icons theme="icons__layout-1" data-aos="fade-up">
    <template v-slot:title>
      <span v-html="pageRodo.data.attributes.sectons_with_icon.section_title"></span>
    </template>

    <template v-slot:icons>
      <div
        v-for="item in pageRodo.data.attributes.sectons_with_icon.icons"
        :key="item.id"
        class="col icon-item"
      >
        <div class="d-flex align-items-center">
          <img :src="item.single_icon_img.data.attributes.url" alt="" />
        </div>
        <span v-html="item.single_icon_txt"></span>
      </div>
    </template>
  </Icons>

  <Icons theme="icons__layout-2" data-aos="fade-up">
    <template v-slot:title>
      <span v-html="pageRodo.data.attributes.section_with_icon_2.section_title"></span>
    </template>

    <template v-slot:icons>
      <div
        v-for="item in pageRodo.data.attributes.section_with_icon_2.icons"
        :key="item.id"
        class="col icon-item"
      >
        <div class="d-flex align-items-center">
          <img :src="item.single_icon_img.data.attributes.url" alt="" />
        </div>
        <span v-html="item.single_icon_txt"></span>
      </div>
    </template>
  </Icons>

  <Cta>
    <template v-slot:col1>
      <div class="col cta-col cta-col-1 text-center">
        <span v-html="pageRodo.data.attributes.CTA.cta_content"></span>
        <router-link class="oan-btn" to="/contact">{{
          pageRodo.data.attributes.CTA.cta_btn_txt
        }}</router-link>
      </div>
    </template>
  </Cta>

  <Footer />
</template>

<script>
import gql from 'graphql-tag'
import Cta from '@/components/Cta.vue'
import Footer from '@/components/Footer.vue'
import GDPRhero from '@/components/GDPRhero.vue'
import Icons from '@/components/Icons.vue'

export default {
  components: {
    Icons,
    GDPRhero,
    Cta,
    Footer
  },
  apollo: {
    pageRodo: gql`
      query {
        pageRodo(locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              page_title
              rodo_header
              RODO_main_icon {
                data {
                  attributes {
                    url
                  }
                }
              }
              sectons_with_icon {
                section_title
                icons {
                  single_icon_txt
                  single_icon_img {
                    data {
                      attributes {
                        url
                      }
                    }
                  }
                }
              }
              section_with_icon_2 {
                section_title
                icons {
                  single_icon_txt
                  single_icon_img {
                    data {
                      attributes {
                        url
                      }
                    }
                  }
                }
              }
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
      pageRodo: {
        data: {
          attributes: {
            page_title: '',
            rodo_header: '',
            RODO_main_icon: {
              data: {
                attributes: {
                  url: ''
                }
              }
            },
            sectons_with_icon: {
              section_title: '',
              icons: [
                {
                  single_icon_txt: '',
                  single_icon_img: {
                    data: {
                      attributes: {
                        url: ''
                      }
                    }
                  }
                }
              ]
            },
            section_with_icon_2: {
              section_title: '',
              icons: [
                {
                  single_icon_txt: '',
                  single_icon_img: {
                    data: {
                      attributes: {
                        url: ''
                      }
                    }
                  }
                }
              ]
            },
            CTA: {
              cta_content: '',
              cta_btn_txt: '',
              has_icon: false,
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
  }
}
</script>
