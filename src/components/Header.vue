<template>
  <div class="hamburger-menu">
    <router-link class="logo d-lg-none" to="/">
      <img src="@/assets/img/logo.png" alt="oan logo"
    /></router-link>
    <button
      @click="toggleMobileMenu"
      class="hamburger hamburger--slider"
      :class="{ 'is-active': store.isMobileMenuVisible }"
      type="button"
    >
      <span class="hamburger-box">
        <span class="hamburger-inner"></span>
      </span>
    </button>
  </div>
  <section :class="{ isMobileMenuVisible: store.isMobileMenuVisible }" class="header">
    <div class="container-fluid">
      <div class="container">
        <div class="row d-flex flex-column flex-lg-row align-items-center">
          <div class="col-12 col-lg-4 d-flex justify-content-center justify-content-lg-start">
            <router-link class="logo d-none d-lg-block" to="/">
              <img src="@/assets/img/logo.png" alt="" data-aos="fade-up"
            /></router-link>
          </div>
          <div class="col-12 col-lg-8 nav--wrapper">
            <nav class="d-flex flex-column flex-lg-row align-items-center" data-aos="fade-up">
              <router-link to="/">{{ lang == 'pl' ? 'Oferta' : 'Offer' }}</router-link>
              <router-link to="/about">{{ lang == 'pl' ? 'O nas' : 'About us' }}</router-link>
              <router-link to="/contact">{{ lang == 'pl' ? 'Kontakt' : 'Contact' }}</router-link>
              <LangSwitcher />
              <router-link class="oan-btn" active-class="no-top-underline" to="/contact">{{
                lang == 'pl' ? 'Napisz do nas' : 'Write to us'
              }}</router-link>
            </nav>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import LangSwitcher from '@/components/LangSwitcher.vue'
import { store } from '@/store'

const lang = localStorage.getItem('lang')

function toggleMobileMenu() {
  store.isMobileMenuVisible = !store.isMobileMenuVisible
}
</script>

<style lang="scss">
@import '@/assets/scss/utils/_variables';
@import '@/assets/scss/utils/_rwd';
@import '@/assets/scss/utils/_mixins';

.lang-switcher {
  span {
    cursor: pointer;
  }
}

.isMobileMenuVisible {
  @include max-lg {
    display: flex !important;
  }
}

.hamburger-menu {
  width: 100%;
  position: fixed;
  left: 0;
  top: 0;
  z-index: 99;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0px 30px 50px 0px;
  background-color: #fff;
  @include lg {
    display: none;
  }

  .logo {
    padding: 15px;
    img {
      max-width: 60px;
    }
  }
}

.header {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  z-index: 20;
  height: 100%;
  background-color: #fff;
  display: none;

  @include lg {
    background-color: #fff;
    height: initial;
    display: flex;
  }

  @include lg {
    padding-top: 26px;
    padding-bottom: 26px;
  }

  .nav--wrapper {
    @include max-lg {
      min-height: 90vh;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }

  nav {
    justify-content: flex-end;
    gap: 15px;
    padding-top: 20px;

    @include lg {
      gap: 24px;
      padding-top: 0;
    }

    @include xl {
      gap: 86px;
      padding-top: 0;
    }

    a {
      font-size: 18px;
      text-decoration: none;
      color: $txt-color;
      position: relative;
    }
  }

  .router-link-active:not(.logo) {
    @include max-lg {
      border-bottom: 2px solid $accent;
      pointer-events: none;
    }
    @include lg {
      &:before {
        content: '';
        width: 52px;
        height: 6px;
        background-color: $accent;
        position: absolute;
        display: block;
        left: 0;
        top: -35px;
        transform: translate(0%, -50%);
      }
    }
  }

  .logo img {
    width: 130px;
  }

  .oan-btn {
    width: 154px;
    padding: 4px 0px;
    text-align: center;
  }
}
</style>
