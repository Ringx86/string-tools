<template>
    <div class="q-pa-md">
        <q-form>
            <div class="q-gutter-y-md column">
                <div class="text-h5">
                    Url 编码转换
                </div>
                <q-input
                    outlined
                    v-model="inputStr"
                    label="输入字符串"
                    type="textarea"
                />
                <div>
                <q-btn-group>
                    <q-btn color="primary" label="URL Decode" @click="decode"/>
                    <q-btn color="primary" label="URL Encode" @click="encode"/>
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

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: '',
  setup () {
    const inputStr = ref('')
    const outputStr = ref('')

    return {
      inputStr,
      outputStr,
      encode () {
        outputStr.value = encodeURI(inputStr.value)
      },
      decode () {
        outputStr.value = decodeURI(inputStr.value)
      }
    }
  }
})
</script>
