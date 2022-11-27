<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import APIShow from './components/APIShow.vue';

import { ref } from 'vue'
import { NMessageProvider, NConfigProvider } from 'naive-ui';
import { NLayout, NLayoutHeader, NLayoutContent, NLayoutSider } from 'naive-ui'; // NLayoutFooter
import { NButton, NMenu, MenuOption, darkTheme, lightTheme, NThemeEditor, NSpace, useMessage } from 'naive-ui';
const themeOverrides = {
  // Button: {
  //   "heightMedium": "42px",
  // }
}
const themeText = ref('light');
const theme = ref(lightTheme);
const apiTitle = ref<string | unknown>('');
const apiDesc = ref({});
const menuOptions: MenuOption[] = [{
  label: '登录注册',
  key: 'loginCase',
  children: [
    {
      label: '检测帐号',
      data: {
        url: '/nop.txt',
        method: 'GET',
        header: [],
        body: ''
      },
      key: 'checkaccount'
    }, {
      label: '请求验证码',
      data: {
        url: '/update',
        method: 'POST',
        header: [],
        body: 'more data'
      },
      key: 'sendcode'
    }
  ]
}];
function changeTheme() {
  if (themeText.value == 'light') {
    themeText.value = 'dark';
    theme.value = darkTheme;
  } else {
    themeText.value = 'light';
    theme.value = lightTheme;
  }
}
function menuChange(key: string, item: MenuOption) {
  console.log(key, item);
  apiTitle.value = item.label;
  apiDesc.value = Object.assign({}, item.data);
}

</script>

<template>
  <n-message-provider>
    <n-config-provider :theme="theme" :theme-overrides="themeOverrides">
      <n-layout position="absolute">
        <n-layout-header bordered>
          <n-space justify="space-around" size="large">
            <n-button type="primary" @click="changeTheme()"> {{ themeText !== "light" ? "浅色" : "深色" }} </n-button>
          </n-space>
        </n-layout-header>
        <n-layout has-sider>
          <n-layout-sider bordered>
            <n-menu :collapsed="false" :on-update:value="menuChange" :options="menuOptions" :collapsed-width="64">
            </n-menu>
          </n-layout-sider>
          <n-layout-content :native-scrollbar="false">
            <n-space>
              <APIShow :title="apiTitle" :data="apiDesc"></APIShow>
            </n-space>
          </n-layout-content>
        </n-layout>
      </n-layout>
    </n-config-provider>
  </n-message-provider>
</template>

<style scoped>
.n-layout-header {
  /* background: rgba(128, 128, 128, 0.2); */
  padding: 8px;
}

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
