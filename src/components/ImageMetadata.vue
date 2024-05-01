<script setup>
import { computed } from 'vue'

const emit = defineEmits(['show-images'])

const props = defineProps(['imageMetadata'])

// TODO: See how to reuse
const imURL = computed(() => {
  return `${import.meta.env.VITE_SEMSEARCH_SVC_URL}/mat/${props.imageMetadata.im_id}`
})

function onShowImages() {
  emit('show-images')
}
</script>

<template>
  <div>
    <img v-bind:src="imURL" class="image-box" />
    <div class="description-card-wrapper">
      <div class="description-card">
        <h2>Captions</h2>
        <p v-for="caption in props.imageMetadata.captions" :key="caption">"{{ caption }}"</p>

        <h2>Dataset</h2>
        <p>{{ props.imageMetadata.dataset }}</p>
        <hr />
        <div id="go-back" @click="onShowImages">GO BACK</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.image-box {
  width: 300px;
  display: inline-block;
  border-style: solid;
  border-color: rgb(82, 76, 61);
  border-width: 2px;
  border-radius: 10px;
}
.description-card-wrapper {
  vertical-align: top;
  margin-top: 0;
  margin-right: 0;
  margin-left: auto;
  margin-bottom: auto;
  width: 496px;
  display: inline-block;
}
.description-card {
  margin-right: 0;
  margin-left: auto;
  width: 450px;
  background-color: white;
  border-style: solid;
  border-color: rgb(82, 76, 61);
  border-width: 2px;
  border-radius: 10px;
  padding: 10px;
}

#go-back {
  width: 100px;
  text-align: center;
  padding-top: 10px;
  height: 30px;
  background-color: rgba(229, 147, 61, 1);
  color: white;
  font-weight: bold;
  font-size: 15px;
  border-radius: 15px;
  border-style: none;
  cursor: pointer;
}
</style>
