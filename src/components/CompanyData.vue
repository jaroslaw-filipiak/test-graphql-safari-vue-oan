<template>
  <section class="company-data">
    <div class="container-fluid">
      <div
        class="container"
        :style="{
          'background-image': 'url(' + require('@/assets/img/company-data-box-bg.png') + ')'
        }"
      >
        <div class="row d-flex flex-column flex-lg-row align-items-center">
          <div class="col company-data-col company-data-col-1">
            <h3>{{ pageContact.data.attributes.column_left_heading }}</h3>
            <p>{{ pageContact.data.attributes.column_left_txt }}</p>
          </div>
          <div class="col company-data-col company-data-col-2">
            <span v-html="pageContact.data.attributes.company_info_column_right"></span>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'
export default {
  name: 'CompanyData',
  apollo: {
    pageContact: gql`
      query {
        pageContact(locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
             column_left_heading
              column_left_txt
              company_info_column_right
            }
          }
        }
      }
    `
  },
  data() {
    return {
      pageContact: {
        data: {
          attributes: {
            column_left_heading: '',
            column_left_txt: '',
            company_info_column_right: ''
          }
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';
@import '@/assets/scss/utils/_mixins';

.company-data {
  .container {
    position: relative;
    margin-top: -74px;
    background-color: #fff;
    padding: 60px 55px 40px 55px;
    @include boxShadow;
    background-repeat: no-repeat;
    background-position: right center;
    background-size: contain;
  }

  &-col-1 {
    p {
      position: relative;
      margin: 40px 0px;

      @include md {
        max-width: 400px;
      }
      &:after {
        content: '';
        width: 21px;
        height: 6px;
        background-color: $accent;
        position: absolute;
        left: 10px;
        top: -20px;
        transform: translate(-50%, 0%);
      }
      &:before {
        content: '';
        width: 21px;
        height: 6px;
        background-color: $accent;
        position: absolute;
        left: 10px;
        bottom: -20px;
        transform: translate(-50%, 0%);
      }
    }
  }
}
</style>
