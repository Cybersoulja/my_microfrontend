<template>
  <h1>// Smmall Uploader</h1>
  <nav>
    <button :class="{ active: view === 'upload' }" @click="view = 'upload'">Upload</button>
    <button :class="{ active: view === 'history' }" @click="view = 'history'">History</button>
  </nav>
  <UploadForm v-if="view === 'upload'" />
  <UploadHistory v-else />
</template>

<script setup>
import { ref } from 'vue'
import UploadForm from './components/UploadForm.vue'
import UploadHistory from './components/UploadHistory.vue'

const view = ref('upload')
</script>
