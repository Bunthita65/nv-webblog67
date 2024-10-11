<template>
  <div class="user-container">
    <h1>Art Work</h1>
    <div class="action-buttons">
      <button @click="navigateTo('/user/create')" class="btn create-btn">Create a user</button>
    </div>
    <hr />
    <div v-if="users.length">
      <div class="user-count">
        <b>Number of users :</b> {{ users.length }}
      </div>
      <div class="user-list">
        <div v-for="user in users" :key="user.id" class="user-card">
          <div class="user-info">
            <div><b>ID:</b> {{ user.id }}</div>
            <div><b>Name:</b> {{ user.name }} {{ user.lastname }}</div>
            <div><b>Gmail:</b> {{ user.email }}</div>
            <div><b>Status:</b> {{ user.status }}</div>
            <div><b>Type:</b> {{ user.type }}</div>
          </div>
          <div class="user-actions">
            <button @click="navigateTo('/user/' + user.id)" class="btn view-btn">See</button>
            <button @click="navigateTo('/user/edit/' + user.id)" class="btn edit-btn">Edit</button>
            <button @click="deleteUser(user)" class="btn delete-btn">Delete</button>
          </div>
        </div>
        <hr />
      </div>
    </div>
    <div class="logout-button">
      <button @click="logout" class="btn logout-btn">Logout</button>
    </div>
  </div>
</template>

<script>
import UsersService from "@/services/UsersService";

export default {
  data() {
    return {
      users: [],
    };
  },
  async created() {
    try {
      this.users = (await UsersService.index()).data;
    } catch (err) {
      console.log(err);
    }
  },
  methods: {
    logout() {
      this.$store.dispatch("setToken", null);
      this.$store.dispatch("setUser", null);
      this.$router.push({
        name: "login",
      });
    },
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(user) {
      let result = confirm("คุณต้องการลบข้อมูลใช่หรือไม่?");
      if (result) {
        try {
          await UsersService.delete(user);
          this.refreshData();
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      try {
        this.users = (await UsersService.index()).data;
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style scoped>
.user-container {
  max-width: 800px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.action-buttons {
  display: flex;
  justify-content: center; /* จัดกลาง */
  margin-bottom: 20px; /* ระยะห่างด้านล่าง */
}

.btn {
  padding: 10px 15px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
  font-weight: bold;
}

.create-btn {
  background-color: #28a745;
  color: white;
}

.create-btn:hover {
  background-color: #218838;
}

.user-count {
  margin-bottom: 10px;
  font-size: 1.2em;
}

.user-list {
  margin-top: 10px;
}

.user-card {
  padding: 15px;
  border: 1px solid #007bff;
  border-radius: 5px;
  margin-bottom: 10px;
  background-color: white;
}

.user-info {
  margin-bottom: 10px;
}

.user-actions {
  display: flex;
  gap: 10px;
}

.view-btn {
  background-color: #007bff;
  color: white;
}

.view-btn:hover {
  background-color: #0056b3;
}

.edit-btn {
  background-color: #ffc107;
  color: black;
}

.edit-btn:hover {
  background-color: #e0a800;
}

.delete-btn {
  background-color: #dc3545;
  color: white;
}

.delete-btn:hover {
  background-color: #c82333;
}

.logout-button {
  text-align: right;
  margin-top: 20px;
}

.logout-btn {
  background-color: #6c757d;
  color: white;
}

.logout-btn:hover {
  background-color: #5a6268;
}
</style>
