<template>
    <div>
        <p class="header-text">{{title}}</p>
    </div>
</template>
<script>
export default {
    name: 'HeaderComponent',

    data() {
        return {
            title: "Book Tracker"
        }
    }

}
</script>
<style> 
    .header-text {
       @apply text-4xl font-bold text-gray-800;
    }
</style>