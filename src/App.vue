<template>
<div>
  <Header />
  <!-- In App.vue -->
  <BookForm :books="books" @book-added="handleBookAdded" />
  <BookList :books="books" />
</div>
</template>

<script>
import Header from './components/Header.vue'
import BookForm from './components/BookForm.vue'
import BookList from './components/BookList.vue'
export default {
  name: 'App',
  components: {
    Header,
    BookForm,
    BookList
  },
  data() {
    return {
      books: []
    };
  },

  methods: {
    handleBookAdded(newBook) {
        this.books.push(newBook);
    }
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  background-color: #2c3e50;
  min-height: 100vh;
}
</style>
