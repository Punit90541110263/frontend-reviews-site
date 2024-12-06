<template>
  <div>
    <h1>Reviews</h1>
    <div v-if="reviews.length > 0">
      <ul>
        <li v-for="(review, index) in reviews" :key="index">
          <h2>{{ review.Title || 'No Title' }}</h2>
          <p>Author: {{ review.author || 'No Author' }}</p>
          <p>Rating: {{ review.rating || 'No Rating' }}</p>
          <p v-if="review.description && review.description.length > 0">
            Description: {{ review.description[0].children[0]?.text || 'No description available.' }}
          </p>
          <p v-else>No description available.</p>
        </li>
      </ul>
    </div>
    <p v-else>No reviews available.</p>
  </div>
</template>


<script>
export default {
  data() {
    return {
      reviews: [], // Initialize reviews array
    };
  },
  async mounted() {
    try {
      const response = await fetch("http://localhost:1337/api/reviews"); // Fetch from Strapi API
      const data = await response.json();
      console.log("Fetched reviews:", data); // Log the fetched data
      if (data && data.data) {
        this.reviews = data.data; // Populate reviews array with fetched data
      } else {
        console.error("Invalid API response format", data);
      }
    } catch (error) {
      console.error("Error fetching reviews:", error);
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
