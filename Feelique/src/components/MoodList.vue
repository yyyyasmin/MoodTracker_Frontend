<script setup>
import { ref, onMounted } from 'vue'

const moods = ref([])
const loading = ref(false)
const error = ref(null)

async function fetchMoods() {
  loading.value = true
  error.value = null

  try {
    const response = await fetch('http://localhost:8080/api/moods')
    if (!response.ok) {
      throw new Error(`HTTP-Fehler: ${response.status}`)
    }

    const data = await response.json()
    moods.value = data
  } catch (err) {
    console.error(err)
    error.value = 'Konnte Moods nicht laden. Ist das Backend gestartet?'
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchMoods()
})
</script>

<template>
  <section>
    <h2>Deine Mood-Einträge</h2>

    <button @click="fetchMoods" :disabled="loading">
      {{ loading ? 'Lade...' : 'Neu laden' }}
    </button>

    <p v-if="error" class="error">{{ error }}</p>

    <p v-if="!loading && moods.length === 0 && !error">
      Noch keine Einträge vorhanden.
    </p>

    <ul v-if="moods.length > 0">
      <li v-for="mood in moods" :key="mood.id" class="mood-item">
        <div class="mood-header">
          <span class="mood-label">{{ mood.mood }}</span>
          <span class="mood-timestamp">
            {{ new Date(mood.timestamp).toLocaleString() }}
          </span>
        </div>
        <p v-if="mood.note" class="mood-note">{{ mood.note }}</p>
      </li>
    </ul>
  </section>
</template>

<style scoped>
h2 {
  margin-top: 0;
}

button {
  margin-bottom: 1rem;
  padding: 0.4rem 0.8rem;
  border-radius: 4px;
  border: 1px solid #ccc;
  cursor: pointer;
}

button:disabled {
  cursor: wait;
  opacity: 0.6;
}

.error {
  color: #b00020;
  font-weight: 500;
}

.mood-item {
  list-style: none;
  padding: 0.8rem 1rem;
  margin-bottom: 0.6rem;
  background: white;
  border-radius: 6px;
  box-shadow: 0 1px 2px rgba(0,0,0,0.08);
}

.mood-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.3rem;
}

.mood-label {
  font-weight: 600;
  text-transform: capitalize;
}

.mood-timestamp {
  font-size: 0.85rem;
  color: #666;
}

.mood-note {
  margin: 0;
}
</style>
