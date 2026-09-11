<script setup>
import { ref, onMounted } from 'vue';

const bookList = ref([])
const user = ref(null)
const newBookTitle = ref('')

onMounted(async () => {
  const storedUser = localStorage.getItem('user')
  if (storedUser) {
    user.value = JSON.parse(storedUser)
  }

  const resp = await fetch('/data/books.json')
  const data = await resp.json()
  bookList.value = data.books
})

function addNewBook() {
  if (!newBookTitle.value.trim() || !user.value) return

  const newBook = {
    bookId: Date.now(), 
    userID: user.value.id,
    bookTitle: newBookTitle.value.trim(),
    status: 'not started'
  }

  bookList.value.push(newBook)

  newBookTitle.value = ''
}

function deleteBook(idToDelete) {
  bookList.value = bookList.value.filter(book => book.bookId !== idToDelete)
}
</script>

<template>
  <section v-if="user">
    <ul>
      <template v-for="book in bookList">
        <li v-if="book.userID === user.id">
          <strong>{{ book.bookTitle }}</strong> — Status: <select v-model="book.status">
            <option value="not started">not started</option>
            <option value="reading">reading</option>
            <option value="finished">finished</option></select>
            <strong class="deleteBookX" v-on:click="deleteBook(book.bookId)" title="Delete Book">X</strong>
        </li>
      </template>
    </ul>
  </section>
  <form @submit.prevent="addNewBook">
      <label for="newBookTitle">New book title </label><input class="addBookInput" v-model="newBookTitle" type="text" placeholder="add book title" required/>
      <button class="addBookBtn" type="submit">Add Book</button>
    </form>
</template>
