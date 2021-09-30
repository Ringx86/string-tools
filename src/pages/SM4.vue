/* eslint-disable @typescript-eslint/no-unsafe-call */
<template>
 <div class="q-pa-md">
  <q-form ref="form">
   <div class="q-gutter-y-md column">
    <div class="text-h5">
      SM4 加解密
   </div>
   <q-input v-model="key" label="输入密钥" outlined :rules="[ val => val && val.length > 0 || 'Required']"/>
   <q-input
    outlined
    v-model="inputStr"
    label="输入字符串"
    type="textarea"
    :rules="[ val => val && val.length > 0 || 'Required']"
    />
    <div>
     <q-btn-group>
      <q-btn color="primary" label="解密" @click="decode"/>
      <q-btn color="primary" label="加密" @click="encode"/>
     </q-btn-group>
    </div>
    <q-input
     outlined
     readonly
     v-model="outputStr"
     type="textarea"
    />
    </div>
  </q-form>
 </div>
</template>

<script>
import { ref } from 'vue'
import JSSM4 from 'jssm4'

export default {
  name: 'sm4-convension',
  setup () {
    const key = ref('')
    const form = ref(null)
    const inputStr = ref('')
    const outputStr = ref('')

    return {
      key,
      form,
      inputStr,
      outputStr,
      async encode () {
        // eslint-disable-next-line @typescript-eslint/no-unsafe-call
        if (await form.value.validate()) {
          const sm4 = new JSSM4(key.value)
          outputStr.value = sm4.encryptData_ECB(inputStr.value)
        }
      },
      async decode () {
        // eslint-disable-next-line @typescript-eslint/no-unsafe-call
        if (await form.value.validate()) {
          const sm4 = new JSSM4(key.value)
          outputStr.value = sm4.decryptData_ECB(inputStr.value)
        }
      }
    }
  }
}
</script>
