<template>
 <div class="q-pa-md">
  <q-form ref="form">
   <div class="q-gutter-y-md column">
    <div class="text-h5">
      Hash
   </div>
   <q-select outlined v-model="hashAlgorithm" :options="hashAlgorithmOptions" label="选择Hash算法"  :rules="[ val => val && val.length > 0 || 'Required']" />
   <q-input
    v-if="needKey()"
    outlined
    v-model="key"
    label="输入Key"
    :rules="[ val => val && val.length > 0 || 'Required']"
    />
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
    const key = ref('')
    const inputStr = ref('')
    const outputStr = ref('')

    const algorithm = {
      md5: 'Md5',
      sha256: 'SHA-256',
      hmac_md5: 'HMAC-Md5',
      hmac_sha256: 'HMAC-Sha256'
    }

    return {
      hashAlgorithm,
      hashAlgorithmOptions: Object.values(algorithm),
      form,
      key,
      inputStr,
      outputStr,
      async hash () {
        if (await form.value.validate()) {
          switch (hashAlgorithm.value) {
            case algorithm.md5:
              outputStr.value = Crypto.MD5(inputStr.value)
              break
            case algorithm.sha256:
              outputStr.value = Crypto.SHA256(inputStr.value)
              break
            case algorithm.hmac_md5:
              outputStr.value = Crypto.HmacMD5(inputStr.value, key.value)
              break
            case algorithm.hmac_sha256:
              outputStr.value = Crypto.HmacSHA256(inputStr.value, key.value)
              break
          }
        }
      },
      needKey(){
        if(hashAlgorithm.value == algorithm.hmac_md5 || hashAlgorithm.value == algorithm.hmac_sha256){
          return true
        }
        else{
          return false
        }
      }
    }
  }
}
</script>
