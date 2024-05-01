<script setup>
import { computed } from 'vue'

const emit = defineEmits(['search-image-metadata'])
const props = defineProps(['imID'])

function onImageMetadataSelected() {
  let requestOptions = {
    method: 'GET',
    redirect: 'follow'
  }

  fetch(`${import.meta.env.VITE_SEMSEARCH_SVC_URL}/images/${props.imID}`, requestOptions)
    .then((response) => response.text())
    .then((result) => emit('search-image-metadata', JSON.parse(result)))
    .catch((error) => console.log('error', error))
}

const imURL = computed(() => {
  return `${import.meta.env.VITE_SEMSEARCH_SVC_URL}/mat/${props.imID}`
})
</script>

<template>
  <div class="image-box">
    <div class="image-thumb-wrapper">
      <div
        class="image-thumb"
        :style="{ 'background-image': 'url(' + imURL + ')' }"
        @click="onImageMetadataSelected"
      ></div>
    </div>
  </div>
</template>

<style scoped>
.image-box {
  width: 200px;
  height: 200px;
  display: inline-block;
  color: white;
}

.image-thumb-wrapper {
  width: 180px;
  height: 180px;
  margin: 9px;
  background-color: black;
  border-style: solid;
  border-width: 2px;
  border-color: rgb(82, 76, 61);
  border-radius: 5px;
}
.image-thumb {
  width: 100%;
  height: 100%;
  background-size: cover;
  cursor: pointer;
}
.image-thumb:hover {
  opacity: 50%;
}
</style>
