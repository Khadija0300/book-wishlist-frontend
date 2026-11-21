
<script setup>
import { ref, onMounted } from 'vue'

const books = ref([])
const error = ref('')

const newBook = ref({
  title: '',
  author: '',
  genre: '',
  priority: 'Mittel',
  purchased: false
})

async function addBook() {
  try {
    error.value = ''

    const response = await fetch(
      'https://book-wishlist-backend.onrender.com/books',
      {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(newBook.value)
      }
    )

    if (!response.ok) {
      throw new Error('Buch konnte nicht gespeichert werden')
    }

    await loadBooks()

    newBook.value = {
      title: '',
      author: '',
      genre: '',
      priority: 'Mittel',
      purchased: false
    }

  } catch (err) {
    error.value = err.message
  }
}


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
    <form @submit.prevent="addBook">
      <input
        v-model="newBook.title"
        placeholder="Buchtitel"
        required
      />

      <input
        v-model="newBook.author"
        placeholder="Autor"
        required
      />

      <input
        v-model="newBook.genre"
        placeholder="Genre"
      />

      <select v-model="newBook.priority">
        <option>Hoch</option>
        <option>Mittel</option>
        <option>Niedrig</option>
      </select>

      <button type="submit">
        Buch hinzufügen
      </button>
    </form>

    <p v-if="error">{{ error }}</p>

    <ul v-else>
      <li v-for="book in books" :key="book.id">
        <strong>{{ book.title }}</strong>
        – {{ book.author }} ({{ book.priority }})
      </li>
    </ul>
  </div>
</template>
