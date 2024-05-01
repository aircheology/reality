<script setup>
import { ref } from 'vue'

import SearchForm from './SearchForm.vue'
import ImageBox from './ImageBox.vue'
import ImageMetadata from './ImageMetadata.vue'

const imIDs = ref([])
const imMetadata = ref(null)

function onSearchImages(value) {
  imIDs.value = value
}
function onSearchImageMetadata(value) {
  imMetadata.value = value
}
function onShowImages() {
  imMetadata.value = null
}
</script>

<template>
  <div>
    <SearchForm class="block" @search-images="onSearchImages" />
    <div v-if="!imMetadata" class="block">
      <ImageBox
        v-for="imID in imIDs"
        :key="imID"
        :imID="imID"
        @search-image-metadata="onSearchImageMetadata"
      />
    </div>
    <ImageMetadata
      v-if="imMetadata"
      class="block"
      :imageMetadata="imMetadata"
      @show-images="onShowImages"
    />
  </div>
</template>

<style scoped>
.block {
  margin-top: 50px;
  width: 800px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>
