<template>
  <div class="blog-show-container">
    <h1>Show Art Work</h1>
    <div class="blog-details">
      <p><strong>ID:</strong> {{ blog.id }}</p>
      <p><strong>Name:</strong> {{ blog.title }}</p>
      <transition name="fade">
        <div v-if="blog.thumbnail" class="thumbnail-pic">
          <img :src="BASE_URL + blog.thumbnail" alt="thumbnail" class="thumbnail" />
        </div>
      </transition>
      <p><strong>Details:</strong> {{ blog.content }}</p>
      <p><strong>Category:</strong> {{ blog.category }}</p>
      <p><strong>Status:</strong> {{ blog.status }}</p>
    </div>
    <div class="action-buttons">
      <button @click="navigateTo('/blog/edit/' + blog.id)" class="edit-button">Edit</button>
      <button @click="navigateTo('/blogs')" class="back-button">Back</button>
    </div>
  </div>
</template>

<script>
import BlogsService from '@/services/BlogsService'

export default {
  data() {
    return {
      blog: null,
    }
  },
  async created() {
    try {
      const blogId = this.$route.params.blogId
      this.blog = (await BlogsService.show(blogId)).data
    } catch (error) {
      console.error("Error fetching blog:", error)
    }
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route)
    },
  }
}
</script>

<style scoped>
.blog-show-container {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

.blog-details {
  margin-bottom: 20px;
}

strong {
  color: #007bff;
}

.thumbnail-pic {
  text-align: center;
  margin: 10px 0;
}

.thumbnail {
  max-width: 100%;
  height: auto;
  border-radius: 5px;
}

.action-buttons {
  display: flex;
  justify-content: space-between;
}

.edit-button,
.back-button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.edit-button {
  background-color: #ffc107;
  color: black;
}

.edit-button:hover {
  background-color: #e0a800;
}

.back-button {
  background-color: #6c757d;
  color: white;
}

.back-button:hover {
  background-color: #5a6268;
}
</style>
