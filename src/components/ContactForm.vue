<template>
  <section class="contact-form" data-aos="fade-up">
    <div class="container-fluid">
      <div class="container box-shadow">
        <div class="row d-flex flex-column flex-lg-row">
          <div class="col col-lg-5 pb-5 pb-lg-0" data-aos="fade-up">
            <h1>
              <span class="underline">{{ pageContact.data.attributes.Title }}</span>
            </h1>
            <p v-html="pageContact.data.attributes.form_section_content"></p>
            <p v-html="pageContact.data.attributes.map_address"></p>
          </div>

          <div class="col col-lg-7 d-flex justify-content-end" data-aos="fade-up">
            <form ref="form" @submit.prevent="onSubmit" data-aos="fade-up">
              <label for="name">
                <input
                  v-model="v$.form.data.name.$model"
                  type="text"
                  id="name"
                  name="name"
                  :placeholder="lang == 'pl' ? 'imię i nazwisko' : 'Name'"
                />
              </label>

              <label for="email" :class="{ error: v$.form.data.email.$errors.length }">
                <input
                  v-model="v$.form.data.email.$model"
                  type="email"
                  id="email"
                  name="email"
                  placeholder="E-mail"
                />

                <div
                  class="input-errors"
                  v-for="(error, index) of v$.form.data.email.$errors"
                  :key="index"
                >
                  <div class="error-msg">{{ error.$message }}</div>
                </div>
              </label>

              <label for="message">
                <textarea
                  v-model="v$.form.data.message.$model"
                  :placeholder="lang == 'pl' ? 'W czym możemy pomóc ?' : 'Your message'"
                  name="message"
                  id="message"
                  cols="30"
                  rows="10"
                ></textarea>
              </label>

              <div class="d-flex align-items-center justify-content-between">
                <input
                  @click="sendData"
                  :disabled="v$.form.data.$invalid"
                  class="oan-btn"
                  type="submit"
                  :value="sendBtn"
                />
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import gql from 'graphql-tag'
import useVuelidate from '@vuelidate/core'
import { required, email } from '@vuelidate/validators'
import { useToast } from 'vue-toastification'
export default {
  name: 'ContactForm',
  apollo: {
    pageContact: gql`
      query {
        pageContact(locale: "${localStorage.getItem('lang') || 'en'}") {
          data {
            attributes {
              Title
              form_section_content
              map_address
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
      pageContact: {
        data: {
          attributes: {
            Title: '',
            form_section_content: '',
            map_address: ''
          }
        }
      },
      form: {
        data: {
          name: '',
          email: '',
          message: ''
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
          },
          name: {
            required
          },
          message: {
            required
          }
        }
      }
    }
  },
  methods: {
    sendData() {
      fetch(`${process.env.VUE_APP_STRAPI_API_URL}api/forms`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.form)
      }).then((response) => {
        console.log(response.status)
        if ((response.status === 200) & (this.lang == 'pl')) {
          this.$refs.form.reset()
          this.toast.success('Dziękujemy za wiadomość')
        } else if ((response.status === 200) & (this.lang == 'en')) {
          this.$refs.form.reset()
          this.toast.success('Thanks for message')
        }
      })
    }
  },
  computed: {
    lang() {
      return localStorage.getItem('lang')
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
@import '@/assets/scss/utils/_mixins';

.contact-form {
  position: relative;
  z-index: 2;

  .container {
    padding: 60px 55px;
    margin-top: 20px;
    background-color: #fff;
  }

  p {
    margin-top: 40px;
    position: relative;

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
  }

  form {
    @include max-lg {
      max-width: 100%;
    }
    max-width: 506px;
    label {
      width: 100%;
    }
    input:not([type='submit']),
    textarea {
      width: 100%;
      margin-bottom: 20px;
      border: 1px solid $gray;
      padding: 5px 15px;
    }
    input[type='submit'] {
      @include max-lg {
        display: inline-flex;
      }

      margin-top: 20px;
    }

    input[type='text'],
    textarea {
      @include lg {
        min-width: 380px !important;
      }
      outline: none;

      transition: all 0.3s ease-in;

      &:focus {
        border: 1px solid $yellow;
        outline: none;
      }
    }
  }
}

.box-shadow {
  @include boxShadow;
}

.input-errors {
  position: absolute;

  .error-msg {
    position: relative;
    font-size: 14px;
    color: $err;
    top: -22px;
  }
}
</style>
