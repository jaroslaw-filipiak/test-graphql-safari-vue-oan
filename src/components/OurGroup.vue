<template>
  <section class="our-group">
    <div class="container-fluid">
      <div class="container">
        <div class="row">
          <div class="col" data-aos="fade-up">
            <h3 class="text-center">
              {{ lang == 'pl' ? 'Grupa Cloud Technologies' : 'Cloud Technologies Group' }}
            </h3>
          </div>
        </div>

        <div class="row pt-5 pb-5">
          <div class="col d-flex justify-content-center">
            <img
              :src="pageContact.data.attributes.our_group.first_item_logo.data.attributes.url"
              alt=""
            />
          </div>
        </div>

        <div class="row d-flex justify-content-center" data-aos="fade-up">
          <div class="col-12 col-lg-9">
            <span v-html="pageContact.data.attributes.our_group.first_item_content"></span>
          </div>
        </div>

        <div class="our-brands row d-flex justify-content-center pb-5">
          <!-- loop -->
          <div
            v-for="item in pageContact.data.attributes.our_group.our_group_items"
            :key="item.id"
            class="our-brands--item col-12 col-lg-9 d-flex flex-column align-items-center flex-lg-row"
            data-aos="fade-up"
          >
            <div class="our-brands--logo">
              <img :src="item.our_group_logo.data.attributes.url" alt="" />
            </div>
            <div class="our-brands--excerpt">
              <span v-html="item.our_group_item_content"></span>
            </div>
          </div>
          <!-- loop END -->
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'
export default {
  name: 'OurGroup',
  apollo: {
    pageContact: gql`
      query {
        pageContact(locale: "${localStorage.getItem('lang') || 'en'}") {
           data {
              attributes {
                our_group {
                  first_item_logo {
                    data {
                      attributes {
                        url
                      }
                    }
                  }
                  first_item_content
                  our_group_items {
                    our_group_item_content
                    our_group_logo {
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
      pageContact: {
        data: {
          attributes: {
            our_group: {
              first_item_logo: {
                data: {
                  attributes: {
                    url: ''
                  }
                }
              },
              first_item_content: '',
              our_group_items: [
                {
                  our_group_item_content: '',
                  our_group_logo: {
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
  },
  computed: {
    lang() {
      return localStorage.getItem('lang')
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';
@import '@/assets/scss/utils/_mixins';

.our-group {
  padding-top: 90px;
}

.our-brands {
  &--item {
    margin-top: 40px;
  }
  &--logo {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    min-width: 30%;
    @include max-lg {
      margin-bottom: 30px;
    }
  }
  &--excerpt {
    padding-left: 30px;
    position: relative;
    @include lg {
      &:before {
        content: '';
        width: 21px;
        height: 6px;
        background-color: $accent;
        position: absolute;
        left: 40px;
        top: -30px;
        transform: translate(-50%, 0%);
      }
    }
  }
}
</style>
