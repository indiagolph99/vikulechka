<script setup lang="ts">
import { ref } from 'vue'
import { read, utils }  from 'xlsx';

defineProps({
  msg: String,
})

const text = ref('')

async function handleFile(e: Event) {
  const target = e.target as HTMLInputElement;
  if (target.files) {
    const file = target.files[0];
    const data = await file.arrayBuffer();
    /* data is an ArrayBuffer */
    const workbook = read(data);

    const jsonData = utils.sheet_to_json(Object.values(workbook.Sheets)[0])
    console.log("Excel as JSON:", jsonData[0])
    text.value = JSON.stringify(jsonData[0])

    /* DO SOMETHING WITH workbook HERE */
  }
}
</script>

<template>
  <div class="card">
    {{ text }}
    <input type="file" name="xlfile" id="xlf" @change="handleFile" />
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
