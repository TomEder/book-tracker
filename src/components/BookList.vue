<template>
  <div>
    <div class="listContainer">
      <ul class="list">
        <li class="listItem" v-for="(book, index) in books" :key="book.id">
          <template v-if="book.isEditing">
            <div class="editModeContainer">
              <!-- Input fields for editing a book -->
              <label for="" class="editModeLabel">Title:</label>
              <input v-model="book.title" class="input" />
              <label for="" class="editModeLabel">Author:</label>
              <input v-model="book.author" class="input" />
              <label for="" class="editModeLabel">Pages:</label>
              <input v-model.number="book.pages" class="input" type="number" />
              <label for="" class="editModeLabel">Pages read:</label>
              <input
                v-model.number="book.pagesRead"
                class="input"
                type="number"
                :max="book.pages"
              />
              <button class="saveButton" @click="saveEditBook(index)">
                Save
              </button>
            </div>
          </template>
          <template v-else>
            <!-- Display fields for a book -->
            <p class="listItemTitle" @click="toggleVisibility(book, $event)">
              {{ book.title }}
            </p>
            <transition name="fade">
              <div
                ref="content"
                class="content"
                :style="{ maxHeight: book.maxHeight }"
              >
                <p class="listItemText">{{ book.author }}</p>
                <p class="listItemText">{{ book.pages }} pages</p>
                <div class="flex">
                  <p class="listItemText">pages read:</p>
                  <input
                    v-model.number="book.pagesRead"
                    class="pagesReadInput"
                    type="number"
                    :max="book.pages"
                  />
                </div>
                <!-- Progress bar -->
                <div class="progress">
                  <div
                    class="progress-bar"
                    :style="{ width: bookProgress(book) + '%' }"
                  >
                    <!-- Show percentage or "Done" based on bookProgress -->
                    {{
                      Math.floor(bookProgress(book)) >= 100
                        ? "Done"
                        : Math.floor(bookProgress(book)) + "%"
                    }}
                  </div>
                </div>
                <button class="removeButton" @click="removeBook(index)">
                  Remove
                </button>
                <button class="editButton" @click="editBook(index)">
                  Edit
                </button>
              </div>
            </transition>
          </template>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "BookList",
  props: {
    books: Array,
  },
  watch: {
    books(newBooks) {
      console.log("Books updated: ", newBooks);

      // Validate and update book.pagesRead
      newBooks.forEach((book) => {
        if (book.pagesRead > book.pages) {
          // If pagesRead is greater than pages, set it to pages
          book.pagesRead = book.pages;
        }
      });
    },
  },
  methods: {
    bookProgress(book) {
      return (book.pagesRead / book.pages) * 100;
    },
    removeBook(index) {
      this.$emit("remove-book", index);
    },
    editBook(index) {
      this.$emit("edit-book", index);
    },
    saveEditBook(index) {
      const updatedBook = { ...this.books[index] };
      this.$emit("save-book", index, updatedBook);
      // No need to set isEditing here, as it's handled in App.vue
    },
    toggleVisibility(book, event) {
      // Ensure there is a next element (safety check)
      const contentElement = event.target.nextElementSibling;
      if (contentElement) {
        if (!book.maxHeight || book.maxHeight === "0px") {
          book.maxHeight = `${contentElement.scrollHeight}px`;
        } else {
          book.maxHeight = "0px";
        }
      }
    },
  },
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.content {
  overflow: hidden;
  transition: max-height 0.5s ease-in-out;
  max-height: 0; /* Start with max-height as 0 */
}

.listContainer {
  @apply flex justify-center bg-gray-100 m-2;
}

.editModeLabel {
  @apply text-lg;
}

.editModeContainer {
  @apply flex flex-col items-center;
}

.saveButton {
  @apply bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded mt-2;
}

.editButton {
  @apply bg-blue-400 hover:bg-blue-700 text-white text-lg ml-2 font-bold py-2 px-4 rounded mt-2;
}

.removeButton {
  @apply bg-red-400 hover:bg-red-700 text-white text-lg font-bold py-2 px-4 rounded mt-2;
}

.pagesReadInput {
  @apply p-1 my-1 border-2 border-black rounded-md bg-slate-200 text-black;
}

.list {
  @apply list-none p-0 w-full;
}

.listItem {
  @apply p-2 my-1 mx-2 text-left border-b-2 border-t-2 rounded-md bg-gray-100 text-black;
}

.listItemTitle {
  @apply text-2xl text-gray-600 font-bold;
}

.listItemText {
  @apply text-lg mt-2 mb-2;
}

.progress {
  @apply flex w-11/12 mt-2 mb-2 bg-gray-300 rounded-md;
}

.progress-bar {
  @apply h-4 bg-green-500 pr-2 pb-5 text-right text-sm text-white rounded transition-all duration-300 w-full;
}

.pagesInput {
  @apply w-20 h-8 mt-2;
}
</style>
