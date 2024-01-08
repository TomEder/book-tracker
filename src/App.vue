<template>
  <div>
    <Header />
    <!-- In App.vue -->
    <BookForm v-if="showModal" :books="books" @book-added="handleBookAdded" @close="showModal = false" />
    <book-list
    :books="books"
    @remove-book="handleBookRemoved"
    @edit-book="handleEditBook"
    @save-book="saveEditBook"
    />
    <button @click="showModal = true" class="addBookButton">+</button>
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
      showModal: false,
    };
  },

  methods: {
    handleBookAdded(newBook) {
      newBook.isEditing = false;
      newBook.isEditing = false; // Initialize the isEditing property
      newBook.isVisible = true; // Initialize the isVisible property
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
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;

  @apply min-h-screen bg-gray-100;
}

.addBookButton {
  @apply bg-green-500 hover:bg-green-700 rounded-full text-2xl text-white font-bold px-4 py-2 fixed bottom-0 right-0 m-4;
}
</style>
