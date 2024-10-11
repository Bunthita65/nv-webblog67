<template>
    <div>
      <h1>Comment Index</h1>
      <div class="comment-section">
        <h2>คอมเมนต์</h2>
        <div class="comments">
          <div v-for="(comment, index) in comments" :key="index" class="comment">
            <p><strong>{{ comment.name }}:</strong> {{ comment.text }}</p>
          </div>
        </div>
        <form @submit.prevent="addComment" class="comment-form">
          <input v-model="newComment.name" type="text" placeholder="ชื่อ" required />
          <input v-model="newComment.text" type="text" placeholder="คอมเมนต์" required />
          <button type="submit">ส่ง</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        comments: [],
        newComment: {
          name: '',
          text: ''
        }
      };
    },
    methods: {
      addComment() {
        if (this.newComment.name && this.newComment.text) {
          this.comments.push({ ...this.newComment });
          this.newComment.name = '';
          this.newComment.text = '';
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .comment-section {
    margin-top: 20px;
  }
  
  .comments {
    margin-bottom: 20px;
  }
  
  .comment {
    border-bottom: 1px solid #ccc;
    padding: 10px 0;
  }
  
  .comment-form {
    display: flex;
    flex-direction: column;
  }
  
  .comment-form input {
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #007bff;
    border-radius: 5px;
  }
  
  .comment-form button {
    padding: 10px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  </style>
  