<template>
  <section class="home-hero" data-aos="fade-up">
    <div class="container-fluid">
      <div class="container">
        <div class="row d-flex flex-column flex-lg-row align-items-center justify-content-between">
          <div class="col text-center text-lg-start">
            <div v-html="pageHomepage.data.attributes.Hero.home_hero_heading"></div>
            <div v-html="pageHomepage.data.attributes.Hero.home_hero_subheading"></div>

            <div class="d-flex flex-column flex-lg-row">
              <NewsletterForm />
            </div>
          </div>

          <div class="col">
            <img
              class="home-hero--icon img-fluid"
              :src="pageHomepage.data.attributes.Hero.hero_img.data.attributes.url"
              alt=""
            />
          </div>

          <div class="flex-column align-items-center d-none d-lg-flex overflow-hidden">
            <img class="home-hero--indicator" src="@/assets/img/section-indicator.png" alt="" />
            <img class="home-hero--indicator-icon" src="@/assets/img/icons/arrow-down.svg" alt="" />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'
import NewsletterForm from '@/components/Forms/NewsletterForm.vue'

export default {
  name: 'Hero',
  components: { NewsletterForm },
  apollo: {
    pageHomepage: gql`
    query getHomepageData {
  pageHomepage(locale: "${localStorage.getItem('lang') || 'en'}") {
    data {
      attributes {
        Hero {
          home_hero_heading
          home_hero_subheading
          hero_img {
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
      pageHomepage: {
        data: {
          attributes: {
            Hero: {
              home_hero_heading: '',
              home_hero_subheading: '',
              hero_img: {
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

<style lang="scss">
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';

.home-hero {
  h1 {
    @include max-lg {
      font-size: 39px;
    }
    @include lg {
      max-width: 473px;
    }
  }

  &--icon {
    position: relative;
    top: -13%;
  }

  &--indicator-icon {
    margin-top: -15px;
    width: 16px;
  }

  h3 {
    font-weight: 400;
    font-size: 1.5rem;
    padding: 50px 0px;
  }
}

h2 {
  font-weight: 400;
  margin-bottom: 45px;
  @include lg {
    max-width: 550px;
  }
}
</style>
