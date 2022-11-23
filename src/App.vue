<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HelloWorld from './components/HelloWorld.vue'

import { ref } from 'vue'
import { NDataTable, NLayoutContent, NLayoutFooter, NLayout, NMessageProvider } from 'naive-ui'
import { NButton, darkTheme, NThemeEditor, NLayoutHeader, NSpace, NConfigProvider, useMessage } from 'naive-ui'
const themeOverrides = {
  Button: {
    "heightMedium": "42px",
  }
}
const theme = ref(null);
</script>

<template>
  <n-message-provider>
    <n-config-provider :theme="theme" :theme-overrides="themeOverrides">
      <n-button type="primary"> 妹妹小 </n-button>
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src="/vite.svg" class="logo" alt="Vite logo" />
        </a>
        <a href="https://vuejs.org/" target="_blank">
          <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
        </a>
      </div>
      <HelloWorld msg="Vite + Vue" />
    </n-config-provider>
  </n-message-provider>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
