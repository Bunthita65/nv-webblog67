<template>
  <div class="blog-container">
    <h2 class="title">Art Work</h2>
    <div class="header-container">
      <button @click="navigateTo('/blog/create')" class="create-button">Create Art Work</button>
      <button @click="logout" class="logout-button">Logout</button>
    </div>
    
    <h4 class="job-count">Number of jobs: {{ blogs.length }}</h4>
    
    <div v-if="loading" class="loading">Loading...</div>
    
    <div v-for="blog in blogs" :key="blog.id" class="blog-card">
      <h5 class="blog-title">{{ blog.title }}</h5>
      <transition name="fade">
        <div v-if="blog.thumbnail !== 'null'" class="thumbnail-pic">
          <img :src="BASE_URL + blog.thumbnail" alt="thumbnail" class="thumbnail" />
        </div>
      </transition>
      <p class="blog-details"><strong>Details:</strong> {{ blog.content }}</p>
      <p class="blog-category"><strong>Category:</strong> {{ blog.category }}</p>
      <p class="blog-status"><strong>Status:</strong> {{ blog.status }}</p>
      <div class="blog-buttons">
        <button @click="navigateTo('/blog/' + blog.id)" class="view-button">
          See
          <img src="https://cdn-icons-png.flaticon.com/128/4230/4230244.png" alt="See the work" class="icon" />
        </button>
        <button @click="navigateTo('/blog/edit/' + blog.id)" class="edit-button">
          Edit
          <img src="https://cdn-icons-png.flaticon.com/128/1159/1159633.png" alt="Edit" class="icon" />
        </button>
        <button @click="deleteBlog(blog)" class="delete-button">
          Delete
          <img src="https://cdn-icons-png.flaticon.com/128/1214/1214428.png" alt="Delete" class="icon" />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import BlogsService from '@/services/BlogsService'

export default {
  data() {
    return {
      blogs: [],
      loading: true
    }
  },
  async created() {
    await this.fetchBlogs()
  },
  methods: {
    async fetchBlogs() {
      try {
        this.loading = true
        this.blogs = (await BlogsService.index()).data
      } catch (error) {
        console.error("Failed to fetch blogs:", error)
        alert("Error loading blogs. Please try again.")
      } finally {
        this.loading = false
      }
    },
    logout() {
      this.$store.dispatch('setToken', null)
      this.$store.dispatch('setBlog', null)
      this.$router.push({ name: 'login' })
    },
    navigateTo(route) {
      this.$router.push(route)
    },
    async deleteBlog(blog) {
      const result = confirm("Do you really want to delete?")
      if (result) {
        try {
          await BlogsService.delete(blog)
          this.fetchBlogs() // Refresh the list after deletion
        } catch (error) {
          console.error("Failed to delete blog:", error)
          alert("Error deleting blog. Please try again.")
        }
      }
    }
  }
}
</script>

<style scoped>
.blog-container {
  max-width: 800px;
  margin: 40px auto;
  padding: 20px;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
}

.title {
  text-align: center;
  color: #333;
  font-size: 2rem;
  margin-bottom: 20px;
}

.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.logout-button,
.create-button {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.logout-button {
  background-color: #dc3545;
  color: white;
}

.logout-button:hover {
  background-color: #c82333;
}

.create-button {
  background-color: #28a745;
  color: white;
}

.create-button:hover {
  background-color: #218838;
}

.job-count {
  text-align: center;
  font-size: 1.2rem;
  color: #555;
}

.blog-card {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 15px;
  margin: 15px 0;
  background-color: #f9f9f9;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
}

.blog-title {
  font-size: 1.5rem;
  color: #007bff;
}

.thumbnail-pic {
  text-align: center;
  margin: 10px 0;
}

.thumbnail {
  width: 100%;
  height: auto;
  border-radius: 5px;
}

.blog-details,
.blog-category,
.blog-status {
  margin: 5px 0;
  font-size: 0.9rem;
  color: #555;
}

.blog-buttons {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}

.view-button,
.edit-button,
.delete-button {
  padding: 8px 12px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
  display: flex;
  align-items: center;
  flex-grow: 1;
  margin: 0 5px; /* Spacing between buttons */
}

.view-button {
  background-color: #007bff;
  color: white;
}

.edit-button {
  background-color: #ffc107;
  color: black;
}

.delete-button {
  background-color: #dc3545;
  color: white;
}

.view-button:hover {
  background-color: #0056b3;
}

.edit-button:hover {
  background-color: #e0a800;
}

.delete-button:hover {
  background-color: #c82333;
}

.icon {
  width: 20px;
  height: 20px;
  margin-left: 8px; /* Space between text and icon */
}

.loading {
  text-align: center;
  font-size: 1.2rem;
  color: #007bff;
  margin: 20px 0;
}
</style>
