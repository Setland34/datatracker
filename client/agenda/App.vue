<template lang="pug">
n-theme
  n-message-provider
    .app-container(ref='appContainer')
      router-view.meeting
</template>

<script setup>
import { onBeforeUnmount ,onMounted, ref } from 'vue'
import { NMessageProvider } from 'naive-ui'

import { useAgendaStore } from './store'

import NTheme from '../components/n-theme.vue'

// STORES

const agendaStore = useAgendaStore()

// STATE

const appContainer = ref(null)

// INIT

agendaStore.fetch()

// --------------------------------------------------------------------
// Handle browser resize
// --------------------------------------------------------------------

const resizeObserver = new ResizeObserver(entries => {
  agendaStore.$patch({ viewport: Math.round(window.innerWidth) })
  // for (const entry of entries) {
    // const newWidth = entry.contentBoxSize ? entry.contentBoxSize[0].inlineSize : entry.contentRect.width
  // }
})

onMounted(() => {
  resizeObserver.observe(appContainer.value, { box: 'device-pixel-content-box' })
})

onBeforeUnmount(() => {
  resizeObserver.unobserve(appContainer.value)
})
</script>

<style lang="scss">
@import "bootstrap/scss/functions";
@import "bootstrap/scss/variables";
@import "../shared/breakpoints";

.meeting {
  > h1 {
    font-weight: 500;
    color: $gray-700;
    display: flex;
    justify-content: space-between;
    align-items: center;

    @media screen and (max-width: $bs5-break-sm) {
      justify-content: center;

      > span {
        font-size: .95em;
      }
    }

    strong {
      font-weight: 700;
      background: linear-gradient(220deg, $blue-500 20%, $purple-500 70%);
      background-clip: text;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      box-decoration-break: clone;
    }
  }

  &-h1-badges {
    display: flex;
    justify-content: end;
    align-items: center;

    > span {
      font-size: 13px;
      font-weight: 700;
      background-color: $pink-500;
      box-shadow: 0 0 5px 0 rgba($pink-500, .5);
      color: #FFF;
      padding: 5px 8px;
      border-radius: 6px;

      & + span {
        margin-left: 10px;
      }
    }
  }

  &-warning {
    background-color: $red-500 !important;
    box-shadow: 0 0 5px 0 rgba($red-500, .5) !important;
    color: #FFF;
    animation: warningBorderFlash 1s ease infinite;
  }

  > h4 {
    @media screen and (max-width: $bs5-break-sm) {
      text-align: center;

      > span {
        font-size: .8em;
        text-align: center;
      }
    }
  }
}
</style>
