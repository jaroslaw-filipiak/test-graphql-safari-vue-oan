<template>
  <form ref="form" @submit.prevent="onSubmit">
    <div
      class="d-flex flex-column flex-md-row flex-md-nowrap align-items-center justify-content-center justify-content-lg-start"
    >
      <div class="position-relative" :class="{ error: v$.form.data.email.$errors.length }">
        <label for="email">
          <input
            @focus="lawInfoVisible = true"
            @blur="lawInfoVisible = false"
            v-model="v$.form.data.email.$model"
            class="home-hero--input"
            type="text"
            name="email"
            id="email"
            :placeholder="placeholder"
          />
        </label>

        <div class="input-errors" v-for="(error, index) of v$.form.data.email.$errors" :key="index">
          <div class="error-msg">{{ error.$message }}</div>
        </div>
      </div>

      <div>
        <label for="submit-btn">
          <input
            @click="sendData"
            :disabled="v$.form.data.$invalid"
            class="oan-btn"
            name="submit-btn"
            id="submit-btn"
            type="submit"
            :value="sendBtn"
          />
        </label>
      </div>
    </div>

    <div v-bind:class="{ 'form--law-info__visible': lawInfoVisible }" class="form--law-info">
      <div id="law-info" v-html="pageHomepage.data.attributes.Hero.newsletter_law_info"></div>
    </div>
  </form>
</template>

<script>
import useVuelidate from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'
import { useToast } from 'vue-toastification'
import gql from 'graphql-tag'

export default {
  name: 'NewsletterForm',
  apollo: {
    pageHomepage: gql`
      query {
        pageHomepage (locale: "${localStorage.getItem('lang') || 'en'}"){
          data {
            attributes {
              Hero {
                newsletter_law_info
              }
            }
          }
        }
      }
    `
  },
  setup() {
    const toast = useToast()
    return { v$: useVuelidate(), toast }
  },

  data() {
    return {
      pageHomepage: {
        data: {
          attributes: {
            Hero: {
              neswletter_law_info: ''
            }
          }
        }
      },
      lawInfoVisible: false,
      form: {
        data: {
          email: ''
        }
      }
    }
  },

  validations() {
    return {
      form: {
        data: {
          email: {
            required,
            email
          }
        }
      }
    }
  },

  methods: {
    sendData() {
      fetch(`${process.env.VUE_APP_STRAPI_API_URL}api/newsletters`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.form)
      }).then((response) => {
        console.log(response.status)
        if ((response.status === 200) & (this.lang == 'pl')) {
          this.$refs.form.reset()
          this.toast.success('Dziękujemy za zapis')
        } else if ((response.status === 200) & (this.lang == 'en')) {
          this.$refs.form.reset()
          this.toast.success('Thanks for subscription')
        }
      })
    },
    togglePicker() {
      this.lawInfoVisible = !this.lawInfoVisible
    }
  },
  computed: {
    lang() {
      return localStorage.getItem('lang')
    },
    placeholder() {
      return this.lang == 'pl' ? 'Adres email' : 'Your email'
    },
    sendBtn() {
      return this.lang == 'pl' ? 'Wyślij' : 'Send'
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';

:global(.home-hero--input) {
  padding: 4px 10px;
  outline: none;
  border: none;
  border: 1px solid $gray;

  @include md {
    padding: 4px 38px;
  }

  @include lg {
    margin-right: 15px;
  }
}

.oan-btn,
.home-hero--input {
  min-height: 35px;

  @include max-md {
    margin-top: 22px;
    width: 246px;
  }

  @include max-lg {
    text-align: center;
  }
}

input[type='text'] {
  @include lg {
    min-width: 380px !important;
  }
  text-align: center;
  transition: all 0.3s ease-in;

  &:focus {
    border-color: $yellow;
  }
}

.form--law-info {
  opacity: 0;
  font-size: 10px;
  margin-top: 30px;
  @include xl {
    max-width: 90%;
  }

  p {
    font-size: 10px !important;
  }

  a {
    color: $yellow;
    &:hover {
      opacity: 0.8;
    }
  }

  &__visible {
    opacity: 1;
  }
}

.input-errors {
  position: absolute;

  .error-msg {
    position: relative;
    font-size: 14px;
    color: $err;
  }
}
</style>
