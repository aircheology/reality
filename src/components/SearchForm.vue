<script setup>
import { ref } from 'vue'

const emit = defineEmits(['search-images'])

const imCount = 10
const unlabeledOff = 'UNLABELED OFF'
const unlabeledOn = 'UNLABELED ON'

const includeUnlabeled = ref(false)
const includeUnlabeledToggleText = ref(unlabeledOff)
const includeUnlabeledToggleOn = ref('include-unlabeled-toggle-on')
const includeUnlabeledToggleOff = ref('include-unlabeled-toggle-off')

const inCaption = ref('')

function onToggleIncludeUnlabeled() {
  includeUnlabeled.value = !includeUnlabeled.value

  if (includeUnlabeled.value) {
    includeUnlabeledToggleText.value = unlabeledOn
  } else {
    includeUnlabeledToggleText.value = unlabeledOff
  }
}

function onSearch() {
  let requestOptions = {
    method: 'GET',
    redirect: 'follow'
  }

  let params = new URLSearchParams()
  params.set('caption', inCaption.value)
  params.set('im_count', imCount)

  if (includeUnlabeled.value) {
    params.set('include_unlabeled', includeUnlabeled.value)
  }

  let url = `${import.meta.env.VITE_SEMSEARCH_SVC_URL}/images?${params.toString()}`

  fetch(url, requestOptions)
    .then((response) => response.text())
    .then((result) => emit('search-images', JSON.parse(result)))
    .catch((error) => console.log('error', error))
}
</script>

<template>
  <form class="search-form">
    <button
      class="include-unlabeled-toggle"
      :class="[includeUnlabeled ? includeUnlabeledToggleOn : includeUnlabeledToggleOff]"
      @click.prevent="onToggleIncludeUnlabeled"
    >
      {{ includeUnlabeledToggleText }}
    </button>
    <input class="caption-input" name="caption" v-model="inCaption" />
    <button class="search-btn" @click.prevent="onSearch">SEARCH</button>
  </form>
</template>

<style scoped>
.search-form {
  background-color: #fff;
  width: 800px;
  height: 50px;
  border-style: solid;
  border-color: rgb(82, 76, 61);
  border-width: 2px;
  border-radius: 25px;
  padding: 0;
}
.caption-input {
  width: 520px;
  height: 34px;
  margin-left: 20px;
  display: inline-block;
  background-color: none;
  border-style: none;
  font-style: bold;
  font-size: 16px;
  font-family: 'Montserrat', sans-serif;
  color: rgb(82, 76, 61);
}
.caption-input:focus {
  outline: none;
}

.include-unlabeled-toggle {
  margin-left: 10px;
  margin-top: 8px;
  width: 140px;
  height: 34px;
  display: inline-block;
  border-style: solid;
  border-color: rgba(166, 194, 84, 1);
  font-weight: bold;
  font-family: 'Montserrat', sans-serif;
  border-width: 2px;
  border-radius: 15px;
  cursor: pointer;
}

.include-unlabeled-toggle-on {
  color: #fff;
  background-color: rgba(166, 194, 84, 1);
}

.include-unlabeled-toggle-off {
  color: rgba(166, 194, 84, 1);
  background-color: #fff;
}

.search-labeled-checkbox {
  width: 10px;
  height: 10px;
  background-color: #edcf55;
  display: inline-block;
}

.search-btn {
  width: 80px;
  margin-left: 20px;
  height: 34px;
  background-color: rgba(229, 147, 61, 1);
  color: white;
  font-weight: bold;
  font-family: 'Montserrat', sans-serif;
  border-radius: 15px;
  border-style: none;
  cursor: pointer;
}
</style>
