<template>
 <div class="q-pa-md">
  <q-form ref="form">
   <div class="q-gutter-y-md column">
    <div class="text-h5">
      Base 64 编码转换
   </div>
   <q-input
    outlined
    v-model="inputStr"
    label="输入字符串"
    type="textarea"
    :rules="[ val => val && val.length > 0 || 'Required']"
    />
    <div>
     <q-btn-group>
      <q-btn color="primary" label="Base64解码" @click="decode"/>
      <q-btn color="primary" label="Base64编码" @click="encode"/>
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
import { toByteArray, fromByteArray } from 'base64-js'
import { Buffer } from 'buffer'

export default {
  name: 'base64-convension',
  setup () {
    const form = ref(null)
    const inputStr = ref('')
    const outputStr = ref('')

    return {
      form,
      inputStr,
      outputStr,
      async encode () {
        // eslint-disable-next-line @typescript-eslint/no-unsafe-call
        if (await form.value.validate()) {
          const bytes = Buffer.from(inputStr.value)
          outputStr.value = fromByteArray(bytes)
        }
      },
      async decode () {
        // eslint-disable-next-line @typescript-eslint/no-unsafe-call
        if (await form.value.validate()) {
          outputStr.value = Buffer.from(toByteArray(inputStr.value)).toString()
        }
      }
    }
  }
}
</script>
