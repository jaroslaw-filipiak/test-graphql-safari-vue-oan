<template>
  <section class="steps">
    <div class="container-fluid">
      <div class="container">
        <div class="row">
          <div class="col text-center">
            <h3 v-html="pageHomepage.data.attributes.Icons.section_title" class="font-600"></h3>
          </div>
        </div>

        <div class="row steps--wrapper">
          <div
            v-for="(item, index) in pageHomepage.data.attributes.Icons.icons"
            :key="index"
            class="col step"
            :class="`step-${index + 1}`"
          >
            <img :src="item.icon_img.data.attributes.url" alt="" />
            <p>{{ item.icon_txt }}</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'
export default {
  name: 'steps',
  apollo: {
    pageHomepage: gql`
      query {
        pageHomepage (locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              Icons {
                section_title
                icons {
                  icon_img {
                    data {
                      attributes {
                        url
                      }
                    }
                  }
                  icon_txt
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
            Icons: {
              section_title: '',
              icons: [
                {
                  icon_img: {
                    data: {
                      attributes: {
                        url: ''
                      }
                    }
                  },
                  icon_txt: ''
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
    },
    placeholder() {
      return this.lang == 'pl' ? 'Adres email' : 'Your email'
    },
    moreBtn() {
      return this.lang == 'pl' ? 'Więcej' : 'More'
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';
@import '@/assets/scss/utils/_mixins';

.steps {
  background-color: $bg-gray;
  margin-bottom: 100px;
  width: 100%;
  max-width: 100%;
  @include max-lg {
    padding-bottom: 75px;
  }

  &--wrapper {
    display: flex;
    flex-direction: column;
    gap: 10px;

    @include lg {
      gap: 20px;
      flex-direction: row;
    }

    @include xl {
      gap: 94px;
      flex-direction: row;
    }
  }

  h3 {
    padding: 75px 0px;
  }

  .step {
    background-color: #fff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    text-align: center;
    padding: 40px 34px;
    position: relative;

    @include boxShadow;
    @include lg {
      top: 30px;
    }

    &:after {
      content: '';
      color: #d8d8d8;
      font-size: 150px;
      position: absolute;
      right: -90px;
      top: 50%;
      font-weight: bold;
      transform: translate(-50%, -50%);
      z-index: 1;
      display: none;
      @include xl {
        display: block;
      }
    }

    &:before {
      content: '';
      background-color: #fff;
      position: absolute;
      right: 0px;
      top: 0;
      width: 100%;
      height: 100%;
      z-index: 2;
    }

    &-1 {
      &:after {
        content: '1';
      }
    }

    &-2 {
      &:after {
        content: '2';
        right: -115px;
      }
    }

    &-3 {
      &:after {
        content: '3';
        right: -100px;
        @include xxl {
          right: -115px;
        }
      }
    }

    p,
    img {
      z-index: 4;
    }

    p {
      font-size: 18px;
      margin: 20px 0px 35px 0px;
      position: relative;

      &:after {
        content: '';
        display: block;
        width: 22px;
        height: 6px;
        background-color: $accent;
        position: absolute;
        left: 50%;
        bottom: -34px;
        transform: translate(-50%, -50%);
      }
    }
  }
}
</style>
