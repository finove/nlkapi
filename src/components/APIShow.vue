<script setup lang="ts">
import { ref } from 'vue'
import { NTime, NDynamicInput, NInputGroup, NInputGroupLabel, NSelect, NCheckbox, NInputNumber, NInput, NButton } from 'naive-ui';
defineProps<{ title: string }>()
const header = ref([{
    key: 'Content-Type',
    value: 'application/json'
}]);
const selectOptions = ref([
    {
        label: 'GET',
        value: 'GET'
    },
    {
        label: 'POST',
        value: 'POST'
    },
    {
        label: 'PUT',
        value: 'PUT'
    },
    {
        label: 'DELETE',
        value: 'DELETE'
    }
]);
</script>
<template>
    <h2>API调试 - {{ title }}</h2>
    <n-time :time="new Date()" />
    <n-input-group>
        <n-select size="medium" default-value="GET" :options="selectOptions" />
        <n-input-group-label>https://api.nlk.com</n-input-group-label>
        <n-input placeholder="url" type="text" />
        <n-button size="medium" type="primary">发送</n-button>
    </n-input-group>
    <n-input-group>
        <n-dynamic-input v-model:value="header" :min="1" preset="pair" key-placeholder="HTTP header key"
            value-placeholder="HTTP header value" />
    </n-input-group>
    <n-input-group>
        <n-input type="textarea" placeholder="没有前后空格" />
    </n-input-group>
</template>
<style scoped>
.n-select{
    width: 164px;
}
.n-input {
    text-align: left;
}
</style>