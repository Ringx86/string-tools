<template>
 <div class="q-pa-md">
  <q-form ref="form">
   <div class="q-gutter-y-md column">
    <div class="text-h5">
      Hash
   </div>
   <q-select outlined v-model="hashAlgorithm" :options="hashAlgorithmOptions" label="选择Hash算法"  :rules="[ val => val && val.length > 0 || 'Required']" />
   <q-input
    outlined
    v-model="inputStr"
    label="输入字符串"
    type="textarea"
    :rules="[ val => val && val.length > 0 || 'Required']"
    />
    <div>
     <q-btn-group>
      <q-btn color="primary" label="hash" @click="hash"/>
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
import Crypto from 'crypto-js'

export default {
  name: 'hash',
  setup () {
    const hashAlgorithm = ref('')
    const form = ref(null)
    const inputStr = ref('')
    const outputStr = ref('')

    const md5Algorithm = 'Md5'
    const sha256Algorithm = 'SHA-256'

    return {
      hashAlgorithm,
      hashAlgorithmOptions: [md5Algorithm, sha256Algorithm],
      form,
      inputStr,
      outputStr,
      async hash () {
        if (await form.value.validate()) {
          switch (hashAlgorithm.value) {
            case md5Algorithm:
              outputStr.value = Crypto.MD5(inputStr.value)
              break
            case sha256Algorithm:
              outputStr.value = Crypto.SHA256(inputStr.value)
              break
          }
        }
      }
    }
  }
}
</script>
