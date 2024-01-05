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
            <p>
                Name: {{ book.title }}
            </p> 
            <p>
                Author: {{ book.author }} 
            </p>
            <p>
                Pages: {{ book.pages }}
            </p>
            <p>
                pages read:
                <input type="number" v-model="book.pagesRead">
            </p>
            <!-- Progress bar -->
            <div class="progress">
                <div class="progress-bar" :style="{ width: bookProgress(book) + '%' }">{{ Math.round(bookProgress(book)) }}%</div>
                
            </div>
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
        }
    },
    methods: {
        bookProgress(book) {
            return book.pagesRead / book.pages * 100;
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
        @apply p-2 my-1 mx-2 border-2 border-black rounded-md bg-slate-300 text-black;
    }

    .progress {
    /* width: 100%; 
    background-color: #e0e0e0; 
    border-radius: 4px;  */

    @apply flex w-11/12 bg-gray-300 rounded-md;
}

.progress-bar {
    height: 1em; /* Height of the progress bar */
    background-color: #4caf50; /* Green background for the filled area */
    text-align: right; /* Aligns the text to the right */
    line-height: 1em; /* Centers the text vertically */
    color: white; /* Text color */
    border-radius: 4px; /* Optional: rounded corners */
    transition: width 0.3s; /* Optional: smooth transition for the bar animation */
}
</style>
