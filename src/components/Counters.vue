<template>
  <span>
    <intersection-observer
      sentinal-name="sentinal-name"
      @on-intersection-element="onIntersectionElement"
    >
    </intersection-observer>
    <section class="counters">
      <div class="container-fluid">
        <div class="container">
          <div class="row">
            <div class="col text-center" data-aos="fade-up">
              <span v-html="pageAbout.data.attributes.Counters.section_title"></span>
            </div>
          </div>
          <div
            v-if="!$apolloData.queries.pageAbout.loading"
            class="row d-flex align-items-center justify-content-center counters--items-wrapper"
          >
            <!-- loop -->
            <div
              v-for="(item, index) in pageAbout.data.attributes.Counters.Counters"
              :key="item.id"
              class="col counter-item"
              data-aos="fade-up"
            >
              <div class="d-flex align-items-center">
                <img :src="item.counter_icon.data.attributes.url" alt="" />
                <div class="counter--value">
                  <number
                    :ref="`item${index}`"
                    :from="10"
                    :to="item.count"
                    :format="theFormat"
                    :duration="3"
                    :delay="0"
                    easing="Power1.easeOut"
                  />
                </div>
              </div>
              <p>{{ item.counter_txt }}</p>
            </div>
            <!-- loop END-->
          </div>
        </div>
      </div>
    </section>
  </span>
</template>

<script>
/* eslint-disable */
import gql from 'graphql-tag'
import VueJsCounter from 'vue-js-counter'
import IntersectionObserver from '@/components/IntersectionObserver'

export default {
  name: 'Counters',
  apollo: {
    pageAbout: gql`
      query {
        pageAbout(locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              Counters {
                section_title
                Counters {
                  count
                  counter_txt
                  counter_icon {
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
      show: true,
      isVisible: true,
      throttle: 0,
      threshold: 0,
      currentInfo: false,
      visible: true,
      isIntersectingElement: false,
      pageAbout: {
        data: {
          attributes: {
            Counters: {
              section_title: '',
              Counters: [
                {
                  count: null,
                  counter_txt: '',
                  counter_icon: {
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
  components: {
    VueJsCounter,
    IntersectionObserver
  },
  methods: {
    async onIntersectionElement(value) {
      this.isIntersectingElement = value
      this.$refs.item0[0].restart()
      this.$refs.item1[0].restart()
      this.$refs.item2[0].restart()
    }
  },
  mounted() {}
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';
@import '@/assets/scss/utils/_mixins';

.counters {
  padding-top: 80px;
  padding-bottom: 80px;

  &--items-wrapper {
    @include lg {
      gap: 150px;
    }
  }

  h3 {
    margin-bottom: 70px;
  }

  .counter-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    max-width: 300px;
    text-align: center;
    border: 1px solid #fff;

    @include max-lg {
      min-width: 80%;
      margin-bottom: 60px;
    }

    .counter--value {
      font-weight: bold;
      font-size: 60px;
      color: #dddddd;

      span {
        display: inline-block;
        min-width: 132px;
      }
    }

    p {
      margin-top: 20px;
      position: relative;

      &:after {
        content: '';
        width: 21px;
        height: 6px;
        background-color: $accent;
        position: absolute;
        left: 50%;
        bottom: -30px;
        transform: translate(-50%, 0%);
      }
    }
  }
}
</style>
