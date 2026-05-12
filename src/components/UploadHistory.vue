<template>
  <div>
    <div v-if="loading" class="spinner">Loading...</div>
    <div v-else-if="error" class="msg error">{{ error }}</div>
    <div v-else-if="records.length === 0" class="history-empty">No uploads yet.</div>
    <div v-else>
      <div v-for="r in records" :key="r.email_id" class="record">
        <div class="record-name">{{ r.filename }}</div>
        <div class="record-meta">
          <span><span class="label">time</span><span class="val">{{ fmt(r.timestamp) }}</span></span>
          <span v-if="r.link"><span class="label">link</span><a class="val" :href="r.link" target="_blank">{{ r.link }}</a></span>
          <span v-if="r.note"><span class="label">note</span><span class="val">{{ r.note }}</span></span>
          <span><span class="label">id</span><span class="val">{{ r.email_id }}</span></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const WORKER = 'https://smmall-uploader.cybersoulja.workers.dev'

const records = ref([])
const loading = ref(true)
const error = ref('')

function fmt(ts) {
  return new Date(ts).toLocaleString()
}

onMounted(async () => {
  try {
    const res = await fetch(`${WORKER}/history`)
    if (!res.ok) throw new Error('Failed to load history')
    records.value = await res.json()
  } catch (e) {
    error.value = e.message
  } finally {
    loading.value = false
  }
})
</script>
