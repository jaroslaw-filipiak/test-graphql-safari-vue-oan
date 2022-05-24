<template>
  <section class="content-page">
    <div class="container-fluid">
      <div class="container">
        <div class="row pb-3">
          <div class="col">
            <h3>{{ pagePrivacyPolicy.data.attributes.page_title }}</h3>
          </div>
        </div>

        <div class="row">
          <div class="col">
            <span v-html="pagePrivacyPolicy.data.attributes.page_content"></span>
          </div>
        </div>
      </div>
    </div>
  </section>
  <Footer />
</template>

<script>
import gql from 'graphql-tag'
import Footer from '@/components/Footer.vue'

export default {
  components: {
    Footer
  },
  apollo: {
    pagePrivacyPolicy: gql`
      query {
        pagePrivacyPolicy(locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              page_title
              page_content
            }
          }
        }
      }
    `
  },
  data() {
    return {
      pagePrivacyPolicy: {
        data: {
          attributes: {
            page_title: '',
            page_content: ''
          }
        }
      }
    }
  }
}
</script>
