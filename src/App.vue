<!-- App.vue -->
<template>
  <div id="app">
    <h1>STM API Messages</h1>
    <div v-if="loading">Loading...</div>
    <ul v-else>
      <li v-for="(message, index) in messages">
        {{ message.id }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      loading: true,
      messages: [],
    };
  },
  mounted() {
    // Fetch data from the API when the component is mounted
    this.fetchMessages();
  },
  methods: {
    async fetchMessages() {
      try {
        // Replace 'YOUR_API_KEY' with your actual API key
        const apiKey = 'l769611a1ecd5547a5b6be663d03f3799b';

        const response = await axios.get(
          "https://api.stm.info/pub/od/gtfs-rt/ic/v2/messages/etatservice",
          {
            headers: {
              'apiKey': apiKey,
            },
          }
        );

        // Log the entire response to the console for debugging
        console.log('API Response:', response);

        // Assuming the API response contains a 'data' property with a 'messages' array
        this.messages = response.data.data.messages || [];
        this.loading = false;
      } catch (error) {
        console.error("Error fetching messages:", error);

        // Log the error response to the console for debugging
        console.log('Error Response:', error.response);
        this.loading = false;
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  font-size: 2em;
  margin-bottom: 20px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 10px 0;
}
</style>
