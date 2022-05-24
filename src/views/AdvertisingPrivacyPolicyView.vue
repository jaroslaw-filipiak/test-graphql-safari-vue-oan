<template>
  <section class="content-page">
    <div class="container-fluid">
      <div class="container">
        <div class="row pb-3">
          <div class="col">
            <h3>{{ pagePrivacyPolicyAdvertising.data.attributes.page_title }}</h3>
          </div>
        </div>

        <div class="row">
          <div class="col">
            <span v-html="pagePrivacyPolicyAdvertising.data.attributes.page_content"></span>
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
    pagePrivacyPolicyAdvertising: gql`
      query {
        pagePrivacyPolicyAdvertising(locale: "${localStorage.getItem('lang') || 'en'}") {
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
      pagePrivacyPolicyAdvertising: {
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
