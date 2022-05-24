<template>
  <section class="gdpr-hero">
    <div class="container-fluid">
      <div class="container">
        <div class="row d-flex flex-column flex-lg-row align-items-center">
          <div
            class="col-12 col-lg-4 d-flex justify-content-center pb-5 pb-lg-0"
            data-aos="fade-up"
          >
            <img :src="pageRodo.data.attributes.RODO_main_icon.data.attributes.url" alt="" />
          </div>
          <div class="col-12 col-lg-8 pl-5 pr-5" data-aos="fade-up">
            <span v-html="pageRodo.data.attributes.page_title"></span>
            <span v-html="pageRodo.data.attributes.rodo_header"></span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'
export default {
  name: 'GDPRhero',
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
            }
          }
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.gdpr-hero {
  padding-bottom: 65px;
  padding-top: 30px;
}
</style>
