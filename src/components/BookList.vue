<template>
    <div>
        <div>
            <h2>
                My books
            </h2>
        </div>
        <div class="listContainer">
    <ul class="list">
        <li class="listItem" v-for="(book, index) in books" :key="index">
            <p class="listItemTitle ">
                {{ book.title }}
            </p> 
            <p class="listItemText">
                {{ book.author }} 
            </p>
            <p class="listItemText">
                {{ book.pages }} pages
            </p>
            <div class="flex">
                <p class="listItemText">
                    pages read:
                </p>
                <input class="pagesInput" type="number" v-model="book.pagesRead" :max="book.pages">
            </div>
            <!-- Progress bar -->
            <div class="progress">
      <div class="progress-bar" :style="{ width: bookProgress(book) + '%' }">
        <!-- Show percentage or "Done" based on bookProgress -->
        {{ Math.floor(bookProgress(book)) >= 100 ? 'Done' : Math.floor(bookProgress(book)) + '%' }}
      </div>
    </div>
    <button class="removeButton" @click="removeBook(index)">Remove</button>
        </li>
    </ul>
</div>
    </div>
</template>

<script>
export default {
    name: 'BookList',
    props: {
        books: Array
    },
    watch: {
        books(newBooks) {
            console.log("Books updated: ", newBooks);
            
            // Validate and update book.pagesRead
            newBooks.forEach(book => {
                if (book.pagesRead > book.pages) {
                    // If pagesRead is greater than pages, set it to pages
                    book.pagesRead = book.pages;
                }
            });
        }
    },
    methods: {
        bookProgress(book) {
            return book.pagesRead / book.pages * 100;
        },
        removeBook(index) {
        this.$emit('remove-book', index);
    }
    }

}
</script>

<style>
    .listContainer {
        @apply flex justify-center bg-slate-500 m-2 border-2 border-black rounded-md;
    }
    
    .list {
        @apply list-none p-0 w-full;
    }

    .listItem {
        @apply p-2 my-1 mx-2 text-left border-2 border-black rounded-md bg-slate-300 text-black;
    }

    .listItemTitle {
        @apply text-2xl font-bold;
    }

    .listItemText {
        @apply text-lg mt-2;
    }

    .removeButton {
        @apply bg-red-500 hover:bg-red-700 text-white text-lg font-bold py-2 px-4 rounded mt-2;
    }

    .progress {
    /* width: 100%; 
    background-color: #e0e0e0; 
    border-radius: 4px;  */

    @apply flex w-11/12 bg-gray-300 rounded-md;
}

.progress-bar {
    @apply h-4 mt-2 bg-green-500 pr-2 pb-5 text-right text-sm text-white rounded transition-all duration-300 w-full;
}


.pagesInput {
    @apply w-20 h-8 mt-2;
}
</style>
