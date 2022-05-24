<template>
  <section class="why-us" data-aos="fade-up">
    <div class="container-fluid">
      <div class="container">
        <div class="row">
          <div class="col text-center" data-aos="fade-up">
            <h3 v-html="pageHomepage.data.attributes.Table.section_title"></h3>
          </div>
        </div>

        <!--table start-->
        <div class="row why-us-table--container">
          <div class="row d-flex justify-content-center">
            <div class="why-us-table--header col-12 col-lg-8">
              <div
                class="why-us-table--row why-us-table--header row d-flex justify-content-center"
                style="width: 100%"
              >
                <div class="col col-4 d-flex align-items-center justify-content-center"></div>
                <div class="col col-4 d-flex align-items-center justify-content-center">
                  {{ pageHomepage.data.attributes.Table.first_column_title }}
                </div>
                <div class="col col-4 d-flex align-items-center justify-content-center">
                  {{ pageHomepage.data.attributes.Table.second_column_title }}
                </div>
              </div>
            </div>
          </div>

          <div class="row d-flex justify-content-center">
            <div class="why-us-table--body col-12 col-lg-8">
              <!-- row-->
              <div
                v-for="item in pageHomepage.data.attributes.Table.table_row_repeater_item"
                :key="item.id"
                class="why-us-table--row row d-flex justify-content-center"
              >
                <div
                  v-html="item.row_title"
                  class="col col-4 border d-flex align-items-center justify-content-start"
                ></div>
                <div
                  class="col col-4 border d-flex align-items-center justify-content-center why-us-table--stars"
                  style="position: relative"
                >
                  <div class="stars--top">
                    <img
                      v-for="star in item.programmatic_model_stars"
                      :key="star.id"
                      src="@/assets/img/icons/star-filled.svg"
                      :alt="star"
                    />
                  </div>
                  <div class="stars--bottom">
                    <img
                      v-for="star in 3"
                      :key="star.id"
                      src="@/assets/img/icons/star-outline.svg"
                      :alt="star"
                    />
                  </div>
                </div>

                <div
                  class="col col-4 border d-flex align-items-center justify-content-center why-us-table--stars"
                  style="position: relative"
                >
                  <div class="stars--top">
                    <img
                      v-for="star in item.classic_model_stars"
                      :key="star.id"
                      src="@/assets/img/icons/star-filled.svg"
                      :alt="star"
                    />
                  </div>
                  <div class="stars--bottom">
                    <img
                      v-for="star in 3"
                      :key="star.id"
                      src="@/assets/img/icons/star-outline.svg"
                      :alt="star"
                    />
                  </div>
                </div>
              </div>
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
  name: 'why-us',
  apollo: {
    pageHomepage: gql`
      query {
        pageHomepage (locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              Table {
                first_column_title
                second_column_title
                section_title
                table_row_repeater_item {
                row_title
                programmatic_model_stars
                classic_model_stars
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
            Table: {
              section_title: '',
              first_column_title: '',
              second_column_title: '',
              table_row_repeater_item: [
                {
                  row_title: '',
                  programmatic_model_stars: 2,
                  classic_model_stars: 2
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

.stars--top,
.stars--bottom {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  min-width: 77px;
  width: 77px;
  max-width: 77px;
  transform: translate(-50%, -50%);
}

.why-us {
  padding-top: 90px;
  padding-bottom: 110px;

  &-table--header {
    background-color: $blue;
    color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    font-weight: 400;
    font-size: 12px;
    padding: 0px 4px;
    @include boxShadow;
    @include lg {
      font-size: 18px;
      font-weight: bold;
      padding: 10px 4px;
    }
  }

  &-table--row {
    .col {
      padding: 10px 5px;
      @include lg {
        height: 53px;
      }
    }
  }

  &-table--stars {
    gap: 3px;
    @include lg {
      gap: 5px;
    }

    img {
      width: 16px;
      @include lg {
        width: 21px;
      }
    }
  }

  &-table--body {
    position: relative;
    @include boxShadow;

    @include max-lg {
      font-size: 12px;
    }

    @include lg {
      &:after {
        content: '';
        display: block;
        width: 36%;
        height: 124%;
        border: 2px solid $yellow;
        position: absolute;
        left: 50%;
        top: -110px;
        border-radius: 10px;
        transform: translate(-50%, 0%);
      }
    }
  }

  h3 {
    margin-bottom: 70px;
  }
}
</style>
