<template>
  <head>
    <link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Thai:wght@100;200;300;400;500;600;700&display=swap" rel="stylesheet">
  </head>
  <div class="login-container">
    <div class="login-box">
      <h2 class="login-title">เข้าสู่ระบบ</h2>
      <div class="user-type-buttons">
        <button 
          :class="{'active': userType === 'student'}" 
          @click="setUserType('student')"
        >
          นักเรียน / นักศึกษา
        </button>
          <button 
              :class="{'active': userType === 'teacher'}" 
              @click="setUserType('teacher')"
          >
              คุณครู / อาจารย์
          </button>
      </div>
      <form @submit.prevent="login" class="login-form">
        <input 
          type="text" 
          v-model="username" 
          placeholder="รหัสประจำตัวคุณครู / อาจารย์" 
          class="login-input"
        />
        <input 
          type="password" 
          v-model="password" 
          placeholder="รหัสประจำตัวคุณครู / อาจารย์" 
          class="login-input"
        />
        <button type="submit" class="login-button">เข้าสู่ระบบ</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      userType: 'teacher',
      username: '',
      password: ''
    };
  },
  methods: {
    setUserType(type) {
      this.userType = type;
      if (type === 'student') {
        this.$router.push('/');
      }
    },
  async login() {
  try {
    // ส่งข้อมูลการเข้าสู่ระบบไปที่ backend
    const response = await axios.post('http://localhost:8000/api/teacher/login', {
      username: this.username,
      password: this.password,
      userType: this.userType
    });

    // ตรวจสอบสถานะและข้อมูลที่ได้รับจาก backend
    if (response.data.message === 'Login successful') {
      // เก็บ username ไว้ใน localStorage
      localStorage.setItem('username', this.username);
      // เปลี่ยนเส้นทางตามประเภทผู้ใช้
      if (this.userType === 'teacher') {
        this.$router.push('/teacher/Home-Teacher');
      } else {
        this.$router.push('/student/Home-Student');
      }
    } else {
      alert('เข้าสู่ระบบไม่สำเร็จ: ' + response.data.message);
    }
  } catch (error) {
    alert('มีข้อผิดพลาดในการเข้าสู่ระบบ: ' + error.response.data.message);
  }
  }
  }
};
</script>

<style>
body {
  background-image: url('/images/background.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  margin: 0;
  padding: 0;
}

.login-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  position: relative;
  z-index: 1;
}

.login-box {
  width: 600px;
  padding: 60px 30px;
  border-radius: 16px;
  background: #FFF;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.login-title {
  font-family: "IBM Plex Sans Thai", sans-serif;
  font-size: 2.5em;
  color: #333;
  margin-bottom: 30px;
}

.user-type-buttons {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}

.user-type-buttons button {
  width: 211px;
  height: 52px;
  flex-shrink: 0;
  border-radius: 15px;
  background: #d3d3d3;
  color: white;
  border: none;
  margin: 0 10px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s, transform 0.2s;
}

.user-type-buttons button.active {
  background-color: #56A7F5;
}

.user-type-buttons button:hover {
  background-color: #56A7F5;
  transform: scale(1.05);
}

.login-form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.login-input {
  width: 528px;
  height: 56px;
  flex-shrink: 0;
  border-radius: 12px;
  border: 1px solid rgba(102, 102, 102, 0.35);
  background: var(--neutral-nr01, #FFF);
  padding: 0 20px;
  margin-bottom: 20px;
  font-size: 1em;
  box-sizing: border-box;
}

.login-input:focus {
  border-color: #5fa3fc;
  outline: none;
  box-shadow: 0 0 5px rgba(95, 163, 252, 0.5);
}

.login-button {
  width: 528px;
  height: 56px;
  border-radius: 12px;
  background-color: #5fa3fc;
  color: white;
  border: none;
  font-size: 1.1em;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.login-button:hover {
  background-color: #4e94e6;
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.2);
}
</style>
