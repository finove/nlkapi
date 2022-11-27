<script setup lang="ts">
import { ref, unref, toRaw, toRefs, toRef } from 'vue';
import { NDynamicInput, NInputGroup, NInputGroupLabel, NSelect, NCheckbox, NInputNumber, NInput } from 'naive-ui';
import { NForm, FormInst, NFormItem } from 'naive-ui';
import { useMessage, NSpace, NButton, NTime } from 'naive-ui';
const props = defineProps<{ title: string | unknown, data: any }>();
const locationURL = window.location.protocol + '//' + window.location.host;
const header = ref([{
    key: 'Content-Type',
    value: 'application/json'
}]);
const message = useMessage();
const apiResponse = ref({ header: '', body: '' });
const formRef = ref<FormInst | null>(null);
const formValue2 = ref({
    url: '/hello',
    method: 'PUT',
    header: [
        {
            key: 'Content-Type',
            value: 'application/json'
        }
    ],
    body: 'some thing'
});
const formValue = toRef(props, 'data');
const rules = {
    url: {
        required: true,
        message: '请输入正确请求地址',
        trigger: ['input', 'blur']
    }
};
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
function sendRequest2(e: MouseEvent) {
    e.preventDefault()
    formRef.value?.validate((errors) => {
        if (!errors) {
            message.success('valid');
            doRequest();
        } else {
            console.log(errors);
            message.error('invalid');
        }
    }).catch((e) => {
        console.log('fail', e);
    });
}

function doRequest() {
    let origin: { url: string, method: string, body: string, header: { key: string, value: string }[] } = toRaw(unref(formValue));
    let header = new Headers();
    // console.log('request data', origin);
    origin.header.forEach(item => {
        header.append(item.key, item.value);
    });
    let myRequest = new Request(origin.url, {
        method: origin.method,
        headers: header,
        body: origin.method != 'GET' ? origin.body : null
    })
    fetch(myRequest).then(response => {
        var headers: string[] = [];
        response.headers.forEach((value, key) => {
            headers.push(key + ": " + value);
        })
        // for (let [key, value] of response.headers) {
        //     headers.push(key + ": " + value);
        // }
        apiResponse.value.header = headers.join('\n');
        return response.text();
    }).then(response => {
        console.log('got response', response);
        apiResponse.value.body = response;
    }).catch(error => {
        message.error("do request fail");
        console.error(error);
    })
}
console.log('props data', props.data);
</script>
<template>
    <h2>API调试 - {{ title }}</h2>
    <n-time :time="new Date()" />
    <n-space>
        <n-form ref="formRef" :label-width="80" :model="formValue" :rules="rules" size="medium">
            <n-form-item path="url">
                <n-input-group>
                    <n-select size="medium" v-model:value="formValue.method" :options="selectOptions" />
                    <n-input-group-label v-show="false">{{ locationURL }}</n-input-group-label>
                    <n-input placeholder="url" type="text" v-model:value="formValue.url" />
                    <n-button size="medium" type="primary" attr-type="button" @click="sendRequest2">发送2</n-button>
                </n-input-group>
            </n-form-item>
            <n-form-item label="请求Header" path="header">
                <n-dynamic-input v-model:value="formValue.header" :min="1" preset="pair"
                    key-placeholder="HTTP header key" value-placeholder="HTTP header value" />
            </n-form-item>
            <n-form-item label="请求Body" path="body">
                <n-input type="textarea" v-model:value="formValue.body" placeholder="body" />
            </n-form-item>
            <pre style="text-align:left;">{{ JSON.stringify(formValue, null, 4) }}</pre>
        </n-form>
        <div>
            <n-form-item label="响应Header">
                <pre style="text-align:left;">{{ apiResponse.header }}</pre>
            </n-form-item>
            <n-form-item label="响应Body">
                <pre style="text-align:left;">{{ apiResponse.body }}</pre>
            </n-form-item>
        </div>
    </n-space>
</template>
<style scoped>
.n-select {
    width: 164px;
}

.n-input {
    text-align: left;
}
</style>