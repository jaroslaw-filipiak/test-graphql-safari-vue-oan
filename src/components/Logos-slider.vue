<template>
  <section class="logos-slider">
    <div class="container-fluid">
      <div class="container">
        <div class="row d-flex flex-xl-nowrap align-items-center justify-content-center">
          <div
            v-for="logo in pageHomepage.data.attributes.Logos.logos"
            :key="logo.id"
            class="logos-slider--logo col col-sm-6 col-xl-2"
          >
            <img :src="logo.image.data.attributes.url" alt="" data-aos="fade-up" />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'
export default {
  name: 'logos-slider',
  apollo: {
    pageHomepage: gql`
      query getLogos {
        pageHomepage {
          data {
            attributes {
              Logos {
                logos {
                  image {
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
      }
    `
  },
  data() {
    return {
      pageHomepage: {
        data: {
          attributes: {
            Logos: [
              {
                image: {
                  data: {
                    attributes: {
                      url: ''
                    }
                  }
                }
              }
            ]
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

.logos-slider {
  margin-top: 80px;
  margin-bottom: 80px;

  &--logo {
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>
