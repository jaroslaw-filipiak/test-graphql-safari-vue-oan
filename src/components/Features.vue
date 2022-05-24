<template>
  <section class="home-features">
    <div class="container-fluid">
      <div class="container">
        <div class="row">
          <div class="col text-center">
            <div
              class="features--heading"
              v-html="pageHomepage.data.attributes.Features.section_title"
            ></div>
          </div>
        </div>

        <div class="row">
          <div class="col text-center features--subtitle">
            <div v-html="pageHomepage.data.attributes.Features.section_subtitle"></div>
          </div>
        </div>

        <div class="row items-row items-row-1">
          <div v-for="item in itemsRow1" v-bind:key="item.id" class="col-6 home-feauters--item">
            <div class="item__icon">
              <img :src="item.feature_item_icon.data.attributes.url" alt="" />
            </div>

            <div class="item__content">
              <h4 v-html="item.feature_item_title"></h4>
              <p v-html="item.feature_item_subtitle"></p>
              <a @click="handleMoreBtn" class="oan-btn" href="#">{{ this.moreBtn }}</a>
            </div>
          </div>
        </div>

        <div class="row items-row items-row-21">
          <div v-for="item in itemsRow2" v-bind:key="item.id" class="col-6 home-feauters--item">
            <div class="item__icon">
              <img :src="item.feature_item_icon.data.attributes.url" alt="" />
            </div>

            <div class="item__content">
              <h4 v-html="item.feature_item_title"></h4>
              <p v-html="item.feature_item_subtitle"></p>
              <a @click="handleMoreBtn" class="oan-btn" href="#">{{ this.moreBtn }}</a>
            </div>
          </div>
        </div>

        <div class="row items-row items-row-3">
          <div v-for="item in itemsRow3" v-bind:key="item.id" class="col-6 home-feauters--item">
            <div class="item__icon">
              <img :src="item.feature_item_icon.data.attributes.url" alt="" />
            </div>

            <div class="item__content">
              <h4 v-html="item.feature_item_title"></h4>
              <p v-html="item.feature_item_subtitle"></p>
              <a @click="handleMoreBtn" class="oan-btn" href="#">{{ this.moreBtn }}</a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'

export default {
  name: 'Features',
  apollo: {
    pageHomepage: gql`
      query {
        pageHomepage (locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              Features {
                section_title
                section_subtitle
                items {
                  feature_item_icon {
                    data {
                      attributes {
                        url
                      }
                    }
                  }
                  feature_item_title
                  feature_item_subtitle
                  feature_item_excerpt
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
            Features: {
              section_title: '',
              section_subtitle: '',
              items: [
                {
                  feature_item_excerpt: '',
                  feature_item_icon: {
                    data: {
                      attributes: {
                        url: ''
                      }
                    }
                  },
                  feature_item_subtitle: '',
                  feature_item_title: ''
                }
              ]
            }
          }
        }
      }
    }
  },
  methods: {
    handleMoreBtn(e) {
      e.preventDefault()
      console.log('handle more btn')
    }
  },
  computed: {
    lang() {
      return localStorage.getItem('lang')
    },
    placeholder() {
      return this.lang == 'pl' ? 'Adres email' : 'Your email'
    },
    moreBtn() {
      return this.lang == 'pl' ? 'Więcej' : 'More'
    },
    itemsRow1() {
      return this.pageHomepage.data.attributes.Features.items.slice(0, 2)
    },
    itemsRow2() {
      return this.pageHomepage.data.attributes.Features.items.slice(2, 4)
    },
    itemsRow3() {
      return this.pageHomepage.data.attributes.Features.items.slice(4, 6)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';

.features {
  &--heading {
    padding-top: 50px;
    padding-bottom: 50px;
  }

  &--subtitle {
    margin-bottom: 44px;
  }
}

.items-row {
  @include lg {
    position: relative;
    &::after {
      content: '';
      width: 400vw;
      height: 1px;
      display: block;
      background: $gray;
      position: absolute;
      left: -100vw;
      bottom: 0;
    }

    &-1 {
      &::before {
        content: '';
        width: 400vw;
        height: 1px;
        display: block;
        background: $gray;
        position: absolute;
        left: -100vw;
        top: 0;
      }
    }
  }
}

.item__content {
  @include lg {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    min-height: 100%;
  }

  .oan-btn {
    width: 133px;
  }
}
.item__icon {
  @include lg {
    display: flex;
    flex-direction: column;
    justify-content: center;
    min-height: 100%;
    padding-right: 20px;
    transform: scale(0.8);
  }

  @include xl {
    padding-right: 0px;
    transform: scale(1);
  }
}

.home-feauters {
  &--item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    padding: 50px;
    border: 1px solid $gray;
    border-top: none;

    &:nth-of-type(even) {
      border-bottom: none;

      @include lg {
        border-right: none;
        border-left: none;
      }
    }

    &:nth-of-type(odd) {
      @include lg {
        border-left: none;
      }
    }

    &:first-child {
      border-top: 1px solid $gray;
    }

    @include lg {
      border-bottom: none !important;
      border-top: none !important;
    }

    @include max-lg {
      width: 100%;
      text-align: center;
    }

    @include lg {
      flex-direction: row;
      align-items: flex-start;
      padding: 25px 5px;
    }

    @include xl {
      padding: 50px;
    }

    h4 {
      margin-bottom: 25px !important;
      margin-top: 25px !important;
      @include lg {
        margin-bottom: 50px !important;
      }
    }
    p {
      @include lg {
        max-width: 75%;
      }
    }
    .oan-btn {
      display: inline-block;
      margin-top: 25px !important;
      @include lg {
        margin-top: 35px !important;
      }
    }
    img {
      @include max-lg {
        transform: scale(0.7);
      }
      @include xl {
        padding-right: 25px;
        padding-top: 30px;
      }
    }
  }
}
h3 {
  margin-bottom: 44px;
}
</style>
