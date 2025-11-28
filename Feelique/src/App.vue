<script setup>
import MoodList from './components/MoodList.vue'
</script>

<template>
  <div class="app-container">
    <header>
      <h1>Mood Tracker</h1>
      <p>Einfaches Frontend, das dein Spring-Backend abfragt.</p>
    </header>

    <main>
      <MoodList />
    </main>
  </div>
</template>

<style scoped>
.app-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

header {
  margin-bottom: 2rem;
}

h1 {
  margin: 0 0 0.5rem;
}

main {
  background: #f5f5f5;
  border-radius: 8px;
  padding: 1.5rem;
}
</style>
