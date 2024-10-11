<template>
  <div class="user-show-container">
    <h1>Show user information</h1>
    <div class="user-info">
      <p><strong>ID:</strong> {{ user.id }}</p>
      <p><strong>Name:</strong> {{ user.name }}</p>
      <p><strong>Lastname:</strong> {{ user.lastname }}</p>
      <p><strong>Email:</strong> {{ user.email }}</p>
      <p><strong>Password:</strong> {{ user.password }}</p>
      <p><strong>Status:</strong> {{ user.status }}</p>
      <p><strong>Type:</strong> {{ user.type }}</p>
      <p><strong>Created At:</strong> {{ user.createdAt }}</p>
    </div>
    <div class="action-buttons">
      <button @click="goBack" class="btn back-btn">กลับ</button>
      <button @click="navigateToEdit" class="btn edit-btn">แก้ไขข้อมูล</button>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      user: {},
    };
  },
  async created() {
    try {
      const userId = this.$route.params.userId;
      this.user = (await UsersService.show(userId)).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    goBack() {
      this.$router.go(-1); // กลับไปยังหน้าก่อนหน้า
    },
    navigateToEdit() {
      this.$router.push(`/user/edit/${this.user.id}`); // ไปยังหน้าแก้ไขข้อมูลผู้ใช้
    },
  },
};
</script>

<style scoped>
.user-show-container {
  max-width: 600px;
  margin: 40px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

.user-info {
  margin-bottom: 20px;
}

strong {
  color: #007bff;
}

.action-buttons {
  display: flex;
  justify-content: space-between;
}

.btn {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.back-btn {
  background-color: #6c757d;
  color: white;
}

.back-btn:hover {
  background-color: #5a6268;
}

.edit-btn {
  background-color: #ffc107;
  color: black;
}

.edit-btn:hover {
  background-color: #e0a800;
}
</style>
