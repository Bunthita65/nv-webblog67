<template>
    <div class="blog-container">
      <h2>Get all blogs</h2>
      <p><button @click="logout" class="logout-button">Logout</button></p>
      <h4>จำนวน blog: {{ blogs.length }}</h4>
      <p>
        <button @click="navigateTo('/blog/create')" class="create-button">สร้าง blog</button>
      </p>
      <div v-for="blog in blogs" :key="blog.id" class="blog-card">
        <h5>{{ blog.title }}</h5>
        <p><strong>Content:</strong> {{ blog.content }}</p>
        <p><strong>Category:</strong> {{ blog.category }}</p>
        <p><strong>Status:</strong> {{ blog.status }}</p>
        <div class="blog-buttons">
          <button @click="navigateTo('/blog/' + blog.id)" class="view-button">ดู blog</button>
          <button @click="navigateTo('/blog/edit/' + blog.id)" class="edit-button">แก้ไข blog</button>
          <button @click="deleteBlog(blog)" class="delete-button">ลบข้อมูล</button>
        </div>
        <hr />
      </div>
    </div>
  </template>
  
  <script>
  import BlogsService from '@/services/BlogsService'
  export default {
    data() {
      return {
        blogs: []
      }
    },
    async created() {
      this.blogs = (await BlogsService.index()).data
    },
    methods: {
      logout() {
        this.$store.dispatch('setToken', null)
        this.$store.dispatch('setBlog', null)
        this.$router.push({ name: 'login' })
      },
      navigateTo(route) {
        this.$router.push(route)
      },
      async deleteBlog(blog) {
        let result = confirm("Want to delete?")
        if (result) {
          try {
            await BlogsService.delete(blog)
            this.refreshData()
          } catch (err) {
            console.log(err)
          }
        }
      },
      async refreshData() {
        this.blogs = (await BlogsService.index()).data
      }
    }
  }
  </script>
  
  <style scoped>
  .blog-container {
    max-width: 800px;
    margin: 40px auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  h2 {
    text-align: center;
    color: #333;
  }
  
  .logout-button {
    background-color: #dc3545;
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .logout-button:hover {
    background-color: #c82333;
  }
  
  .create-button {
    background-color: #28a745;
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .create-button:hover {
    background-color: #218838;
  }
  
  .blog-card {
    border: 1px solid #ddd;
    border-radius: 5px;
    padding: 15px;
    margin: 15px 0;
    background-color: white;
    box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);
  }
  
  .blog-buttons {
    display: flex;
    justify-content: space-between;
  }
  
  .view-button,
  .edit-button,
  .delete-button {
    padding: 8px 12px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
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
  </style>
  