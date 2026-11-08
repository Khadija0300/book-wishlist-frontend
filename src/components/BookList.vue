
<script setup>
import { ref } from 'vue'

const books = ref([
  { id: 1, title: 'Harry Potter', author: 'J. K. Rowling' },
  { id: 2, title: 'Die Tribute von Panem', author: 'Suzanne Collins' },
  { id: 3, title: 'Das Café am Rande der Welt', author: 'John Strelecky' },
])
</script>

<template>
  <div>
    <h1>Meine Bücher-Wunschliste 📚</h1>

    <ul>
      <li v-for="book in books" :key="book.id">
        <strong>{{ book.title }}</strong>
        – {{ book.author }}
      </li>
    </ul>
  </div>
</template>
