
<script setup>
import { ref, onMounted } from 'vue'

const books = ref([])
const error = ref('')

async function loadBooks() {
  try {
const response = await fetch('https://book-wishlist-backend.onrender.com/books')
    if (!response.ok) {
      throw new Error('Bücher konnten nicht geladen werden')
    }

    books.value = await response.json()
  } catch (err) {
    error.value = err.message
  }
}

onMounted(() => {
  loadBooks()
})
</script>

<template>
  <div>
    <h1>Meine Bücher-Wunschliste 📚</h1>

    <p v-if="error">{{ error }}</p>

    <ul v-else>
      <li v-for="book in books" :key="book.id">
        <strong>{{ book.title }}</strong>
        – {{ book.author }} ({{ book.priority }})
      </li>
    </ul>
  </div>
</template>
