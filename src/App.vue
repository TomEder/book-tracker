<template>
  <div>
    <Header />
    <!-- In App.vue -->
    <BookForm :books="books" @book-added="handleBookAdded" />
    <book-list
      :books="books"
      @remove-book="handleBookRemoved"
      @edit-book="handleEditBook"
      @save-book="saveEditBook"
    />
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import BookForm from "./components/BookForm.vue";
import BookList from "./components/BookList.vue";
export default {
  name: "App",
  components: {
    Header,
    BookForm,
    BookList,
  },
  data() {
    return {
      books: [],
    };
  },

  methods: {
    handleBookAdded(newBook) {
      newBook.isEditing = false; // Initialize the isEditing property
      this.books.push(newBook);
    },
    handleBookRemoved(index) {
      this.books.splice(index, 1);
    },
    handleEditBook(index) {
      this.books.forEach((book, i) => {
        book.isEditing = i === index; // Directly set isEditing
      });
    },
    saveEditBook(index, updatedBook) {
      this.books[index] = { ...updatedBook, isEditing: false }; // Directly update the book
    },
  },
};
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
