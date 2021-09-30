/* eslint-disable @typescript-eslint/no-unsafe-call */
<template>
 <div class="q-pa-md">
  <q-form ref="form">
   <div class="q-gutter-y-md column">
    <div class="text-h5">
      AES/DES 加解密
   </div>
   <q-select outlined v-model="algorithm" :options="algorithmOptions" label="选择算法"  :rules="[ val => val && val.length > 0 || 'Required']" />
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
import Crypto from 'crypto-js'

export default {
  name: 'sm4-convension',
  setup () {
    const form = ref(null)
    const key = ref('')
    const algorithm = ref('')
    const inputStr = ref('')
    const outputStr = ref('')
    
    const algorithmType = {
      aes: 'AES',
      des: 'DES',
    }

    return {
      key,
      form,
      algorithm,
      key,
      inputStr,
      outputStr,
      algorithmOptions:Object.values(algorithmType),
      async encode () {
              
        if (await form.value.validate()) {     
          switch(algorithm.value){
              case algorithmType.aes:
                  outputStr.value = Crypto.AES.encrypt(inputStr.value, key.value).toString()
                  break
              case algorithmType.des:
                  outputStr.value = Crypto.DES.encrypt(inputStr.value, key.value).toString()
                  break
          }  
        }
      },
      async decode () {
        if (await form.value.validate()) {
          switch(algorithm.value){
              case algorithmType.aes:
                  outputStr.value = Crypto.AES.decrypt(inputStr.value, key.value).toString(Crypto.enc.Utf8)
                  break
              case algorithmType.des:
                  outputStr.value = Crypto.DES.decrypt(inputStr.value, key.value).toString(Crypto.enc.Utf8)
                  break
          }  
        }
      }
    }
  }
}
</script>
