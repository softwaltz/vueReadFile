<script setup>
import { ref } from 'vue'

const imgs = ref([])
const jsons = ref([])

const onFileChange = (event) => {
  for (var f of event.target.files) {
    const fileReader = new FileReader()
    let tmpName = ''
    tmpName = f.name.split('.')

    if (tmpName[tmpName.length - 1] == 'jpg') {
      fileReader.addEventListener(
        'load',
        () => {
          let image = {
            name: f.name,
            url: fileReader.result
          }
          imgs.value.push(image)
        },
        false
      )
      fileReader.readAsDataURL(f)
    } else if (tmpName[tmpName.length - 1] == 'json') {
      fileReader.addEventListener(
        'load',
        () => {
          let j = fileReader.result
          let json = {
            name: f.name,
            content: JSON.parse(j)
          }
          jsons.value.push(json)
        },
        false
      )
      console.log(f)
      fileReader.readAsText(f)
    }
  }
}
</script>

<template>
  <input type="file" @change="onFileChange" multiple />
  <br />
  <div v-for="(val, idx) in imgs" :key="idx">
    {{ idx + 1 }}
    <img :src="val.url" style="width: 100px" />
    name: {{ val.name }}
  </div>
  <div v-for="(val, idx) in jsons" :key="idx">{{ idx + 1 }}, json:{{ val }}</div>
</template>
