<template>
    <div class="classes-component">
        <div class="search-bar">
            <div class="search-input">
                <input v-model="searchTerm" placeholder="Search Lessons.." @input="searchClasses">
            </div>
        </div>
        <ul>
            <!-- Filtered lessons -->
            <li v-for="lesson in filteredLessons" :key="lesson.id">
                <div class="lesson-info">
                    <img :src="getLessonImage(lesson.title)" :alt="lesson.title + ' Icon'" class="lesson-icon">
                    <br>
                    <span>
                        <strong>{{ lesson.title }}</strong>
                        <br>
                        Location: <strong>{{ lesson.location }}</strong>
                        <br>
                        Price: <strong>£{{ lesson.price }}</strong>
                        <br>
                        Spaces: <strong>{{ lesson.spaces }}</strong>
                    </span>
                </div>
                <button @click="addToCart(lesson)" :disabled="lesson.spaces === 0">Add to Cart</button>
            </li>
        </ul>
    </div>
</template>
  
<script>
export default {
    props: ['lessons'],
    data() {
        return {
            searchTerm: '',
            searchResults: [],
        };
    },
    computed: {
        // Filter and sort lessons
        filteredLessons() {
            if (this.searchTerm.trim() !== '') {
                return this.searchResults;
            }
            return this.lessons.filter((lesson) => {
                const lowerCaseSearchTerm = this.searchTerm.toLowerCase();
                return (
                    lesson.title.toLowerCase().includes(lowerCaseSearchTerm) ||
                    lesson.location.toLowerCase().includes(lowerCaseSearchTerm)
                );
            });
        },
    },
    methods: {
        // Search classes based on search term
        searchClasses() {
            const lowerCaseSearchTerm = this.searchTerm.toLowerCase();
            this.searchResults = this.lessons.filter((lesson) => {
                return (
                    lesson.title.toLowerCase().includes(lowerCaseSearchTerm) ||
                    lesson.location.toLowerCase().includes(lowerCaseSearchTerm)
                );
            });
        },
        // Get lesson image based on title
        getLessonImage(title) {
            const imgMap = {
                Maths: "math-icon.svg",
                History: "history-icon.svg",
                Science: "science-icon.svg",
                Art: "art-icon.svg",
                Music: "music-icon.svg",
                Programming: "programming-icon.svg",
                English: "english-icon.svg",
                PE: "PE-icon.svg",
                Chemistry: "chemistry-icon.svg",
                Geography: "geography-icon.svg",
            };
            return "https://afterschoolapp-env.eba-suztb7mk.eu-west-2.elasticbeanstalk.com/images/" + imgMap[title] || "";
        },
        // Add a lesson to the cart
        addToCart(lesson) {
            this.$emit('add-to-cart', lesson);
        },
    },
};
</script>
  
<style scoped>
/* Add your styles here */
</style>
  