<template lang="">
  <div class="modalBackground" @click.self="closeModal">
  <div class="formContainer">
    <h4 class="formHeader">Add a book</h4>
    <button @click="closeModal" class="closeButton">X</button>
    <form class="inputContainer" @submit.prevent="addBook" action="">
      <input
        class="formInput"
        type="text"
        placeholder="Title"
        v-model="newBook.title"
      />
      <input
        class="formInput"
        type="text"
        placeholder="Author"
        v-model="newBook.author"
      />
      <input
        class="formInput"
        type="number"
        placeholder="How many pages?"
        v-model="newBook.pages"
      />
      <button class="formButton" type="submit">Add</button>
    </form>
  </div>
</div>
</template>
<script>
export default {
  name: "BookForm",
  data() {
    return {
      newBook: {
        title: "",
        author: "",
        pages: 0,
      },
    };
  },
  methods: {
    addBook() {
      const book = {
        id: Date.now(), // Use a timestamp as a simple unique ID
        title: this.newBook.title,
        author: this.newBook.author,
        pages: this.newBook.pages,
        pagesRead: 0, // Initialize pagesRead
        isEditing: false, // Initialize isEditing
        isVisible: true, // Initialize isVisible
      };

      this.$emit("book-added", book); // Emit an event to the parent

      // Reset the form fields
      this.newBook.title = "";
      this.newBook.author = "";
      this.newBook.pages = 0;
      this.$emit("close");
    },
    closeModal() {
    console.log("cloing modal")
    this.$emit('close');
  },
  },
};
</script>
<style>
.modalBackground {
  @apply fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-10;
}

.formContainer {
  @apply bg-white p-4 rounded-lg;
}

.closeButton {
  @apply absolute top-0 right-0 m-2 text-lg;
}

.formContainer {
  @apply bg-gray-100 m-2 border rounded-md;
  @apply relative;
}
.formHeader {
  @apply text-sm font-bold;
}
.inputContainer {
  @apply flex flex-col p-2;
}
.formInput {
  @apply p-1 my-1 border-2 border-black rounded-md bg-slate-300 text-black;
}
.formButton {
  @apply p-1 my-1 border-2 w-12 m-auto border-black rounded-md bg-green-300;
}
</style>
