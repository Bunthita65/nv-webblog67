<template>
  <div class="user-login">
    <h1 v-if="!isLogin">Login</h1>
    <h1 v-else>Create User</h1>
    
    <form @submit.prevent="isLogin ? createUser() : loginUser()">
      <div>
        <label>Email:</label>
        <input type="email" v-model="email" required />
      </div>
      <div>
        <label>Password:</label>
        <input type="password" v-model="password" required />
      </div>
      <button type="submit">{{ isLogin ? 'Create User' : 'Login' }}</button>
    </form>

    <p @click="toggleForm">{{ isLogin ? 'Already have an account? Login' : 'Don’t have an account? Create one' }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      isLogin: true // กำหนดให้เริ่มต้นอยู่ในโหมด login
    };
  },
  methods: {
    toggleForm() {
      this.isLogin = !this.isLogin; // สลับโหมดระหว่าง Login และ Create User
    },
    loginUser() {
      // ฟังก์ชันสำหรับเข้าสู่ระบบ
      // รหัสเข้าสู่ระบบของคุณจะอยู่ที่นี่
    },
    createUser() {
      // ฟังก์ชันสำหรับสร้างผู้ใช้
      // รหัสสร้างผู้ใช้ของคุณจะอยู่ที่นี่
    }
  }
};
</script>

<style scoped>
.user-login {
  max-width: 400px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
}

.user-login h1 {
  text-align: center;
}

.user-login form {
  display: flex;
  flex-direction: column;
}

.user-login label {
  margin-bottom: 5px;
}

.user-login input {
  margin-bottom: 15px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.user-login button {
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: white;
  font-weight: bold;
}

.user-login button:hover {
  background-color: #0056b3;
}

.user-login p {
  text-align: center;
  color: #007bff;
  cursor: pointer;
}
</style>

<template>
  <div>
    <div class="nv-navbar">
      <ul class="nav">
        <li><router-link :to="{ name: 'comments' }">Comment</router-link></li>
        <li><router-link :to="{ name: 'blogs' }">Art Work</router-link></li>
        <li><router-link :to="{ name: 'users' }">Create User</router-link></li>
        <li><router-link :to="{ name: 'login' }">Login</router-link></li>  
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    isLogin() {
      return this.$store.getters.isUserLoggedIn; // เช็คการเข้าสู่ระบบ
    },
    logout() {
      this.$store.dispatch('logout');
      this.$router.push({ name: 'login' }); // ไปยังหน้า Login หลังจากออกจากระบบ
    }
  }
};
</script>

<style scoped>
.nv-navbar {
  background-color: rgb(0, 0, 0);
  width: 100%;
  padding: 10px 20px; /* เพิ่ม padding ด้านข้าง */
  display: flex; /* ใช้ flexbox เพื่อจัดการการแสดงผล */
  justify-content: space-between; /* ช่องว่างระหว่างลิงก์ซ้ายและขวา */
  align-items: center; /* จัดแนวให้กลางแนวนอน */
}

.nav {
  list-style: none; /* ลบจุดหรือตัวเลขจากลิสต์ */
  margin: 0;
  padding: 0;
  display: flex; /* ใช้ flexbox เพื่อจัดระเบียบลิงก์ */
}

.nav li {
  margin-right: 20px; /* เพิ่มระยะห่างระหว่างลิงก์ */
}

.nav li:last-child {
  margin-right: 0; /* ลบระยะห่างด้านขวาสำหรับลิงก์สุดท้าย */
}

.nav li a {
  padding: 10px 15px; /* เพิ่ม padding ด้านใน */
  text-decoration: none; /* ลบเส้นใต้ */
  color: rgb(255, 255, 255); /* สีตัวอักษร */
  font-weight: bold; /* ตัวหนา */
  border-radius: 5px; /* มุมมน */
  transition: background-color 0.3s, color 0.3s; /* เพิ่มการเปลี่ยนสีอย่างนุ่มนวล */
}

.nav li a:hover {
  background-color: rgb(50, 50, 50); /* สีพื้นหลังเมื่อเอาเมาส์ไปวาง */
  color: rgb(192, 192, 192); /* เปลี่ยนสีฟอนต์เมื่อเอาเมาส์ไปวาง */
}

.nav li a.router-link-active {
  background-color: rgb(255, 255, 255); /* สีพื้นหลังเมื่อเป็นลิงก์ที่ถูกเลือก */
  color: darkslategrey; /* สีฟอนต์เมื่อเป็นลิงก์ที่ถูกเลือก */
}
</style>
