<template>
  <form @submit.prevent="submit">
    <div>
      <label>File</label>
      <input type="file" @change="onFile" required />
    </div>
    <div>
      <label>Link (optional)</label>
      <input type="url" v-model="link" placeholder="https://..." />
    </div>
    <div>
      <label>Note (optional)</label>
      <textarea v-model="note" placeholder="Any notes..."></textarea>
    </div>
    <button class="submit" :disabled="!file || loading">
      {{ loading ? 'Uploading...' : 'Upload' }}
    </button>
    <div v-if="msg" :class="['msg', msgType]">{{ msg }}</div>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const WORKER = 'https://smmall-uploader.cybersoulja.workers.dev'

const file = ref(null)
const link = ref('')
const note = ref('')
const loading = ref(false)
const msg = ref('')
const msgType = ref('success')

function onFile(e) {
  file.value = e.target.files[0] || null
}

async function submit() {
  loading.value = true
  msg.value = ''
  const fd = new FormData()
  fd.append('file', file.value)
  if (link.value) fd.append('link', link.value)
  if (note.value) fd.append('note', note.value)
  try {
    const res = await fetch(`${WORKER}/upload`, { method: 'POST', body: fd })
    const data = await res.json()
    if (!res.ok) throw new Error(data.error || 'Upload failed')
    msg.value = `Sent: ${data.file} (id: ${data.email_id})`
    msgType.value = 'success'
    link.value = ''
    note.value = ''
    file.value = null
  } catch (e) {
    msg.value = e.message
    msgType.value = 'error'
  } finally {
    loading.value = false
  }
}
</script>
