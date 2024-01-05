<template lang="">
    <div class="formContainer">
        <h4 class="formHeader">Add a book</h4>
        <form class="inputContainer" @submit.prevent="addBook" action="">
            <input class="formInput" type="text" placeholder="Title" v-model="newBook.title">
            <input class="formInput" type="text" placeholder="Author" v-model="newBook.author">
            <input class="formInput" type="number" placeholder="How many pages?" v-model="newBook.pages">
            <button class="formButton" type="submit">Add</button>
        </form>
    </div>
</template>
<script>
export default {
    name: 'BookForm',
    data() {
        return{
            newBook: {
                title: '',
                author: '',
                pages: 0
            }
        }
    },
    methods: {
    addBook() {
        const book = {
            title: this.newBook.title,
            author: this.newBook.author,
            pages: this.newBook.pages
        };

        this.$emit('book-added', book); // Emit an event to the parent

        // Reset the form fields
        this.newBook.title = '';
        this.newBook.author = '';
        this.newBook.pages = 0;
    }
}
    
}
</script>
<style>
    .formContainer {
        @apply bg-slate-500 m-2 border-2 border-black rounded-md;
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