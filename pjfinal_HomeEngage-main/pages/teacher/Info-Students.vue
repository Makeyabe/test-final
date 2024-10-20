<template>
  <Head>
    <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
  </Head>
  <div class="container">
    <SidebarTeacher />

    <div class="main-content">
      <div class="header">
        <h1>HOME ENGAGE</h1>
      </div>
      <div class="content-area">
        <div class="info-box">
          <span class="info-text">ข้อมูลนักเรียน</span>
          <router-link to="/teacher/ProfileStudent">
            <button class="back" @click="toggleEdit">ย้อนกลับ</button>
          </router-link>
        </div>
        <div class="button-container">
          <div class="button-group">
            <button class="toggle-button" :class="{ active: isActive === 'info' }" @click="goToProfile">ข้อมูล</button>
            <button class="toggle-button" :class="{ active: isActive === 'map' }" @click="goToMap">แผนที่บ้าน</button>
          </div>
        </div>
        <StudentDetails :isEditing="isEditing" />
      </div>
    </div>
  </div>
</template>

<script setup>
import SidebarTeacher from '/pages/components/SidebarTeacher.vue';
import StudentDetails from '/pages/components/StudentDetails.vue';
import { useRouter } from 'vue-router';
import { ref } from 'vue';

const router = useRouter();
const isActive = ref('info');

const goToMap = () => {
  isActive.value = 'map';
  router.push('/teacher/Mapstudents').catch(() => {}); // เพิ่มการจับข้อผิดพลาด
};

const goToProfile = () => {
  isActive.value = 'info';
  router.push('/teacher/Info-students').catch(() => {}); // เพิ่มการจับข้อผิดพลาด
};
</script>
  
<style scoped>
  .container {
    display: flex;
    height: 111vh;
    font-family: 'Inter', sans-serif;
  }
  
  .main-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    background-color: #f5f5f5;
  }
  
  .header {
    background-color: #56A7F5;
    padding: 5px 20px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    height: 80px;
  }
  
  .header h1 {
    font-size: 42px;
    font-weight: 400;
    color: white;
    font-family: "Chewy", system-ui;
    margin-left: 20px;
  }
  
  .content-area {
    flex: 1;
    background-color: #F9F9F9;
    margin: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }

  .info-box {
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: #ECECEC; /* Adjust this color to match the background */
    border-radius: 8px 8px 0 0; /* มุมมนเฉพาะด้านบน */
    padding: 12px 20px;
    font-size: 16px;
    color: #333;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .info-text {
        font-size: 18px;
        font-weight: 500;
        color: #333;
  }

  .back {
        background-color: #56A7F5; /* Matches the edit button color */
        color: white;
        border: none;
        font-size: 14px;
        padding: 8px 16px;
        border-radius: 10px;
        cursor: pointer;
        transition: background-color 0.3s ease;
  }

  .back:hover {
        background-color: #72b6f6; /* Hover effect color */
  }
  .button-container {
    display: flex;
    justify-content: center; /* จัดตรงกลางในแนวนอน */
    align-items: flex-start; /* ชิดด้านบน */
    padding-top: 20px; /* เพิ่มระยะห่างจากด้านบน */
    width: 100%; /* ให้คอนเทนเนอร์ครอบคลุมความกว้างเต็ม */
  }

.button-group {
  display: flex;
  gap: 30px; /* ระยะห่างระหว่างปุ่ม */
}

.toggle-button {
  text-decoration: none;
  display: inline-block;
  width: 120px;
  height: 40px;
  line-height: 40px; /* จัดข้อความให้อยู่กลางแนวตั้ง */
  border: none;
  border-radius: 15px;
  font-size: 18px;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
  outline: none;
  color: #56A7F5;
  background-color: #ECECEC;
  text-align: center; /* จัดข้อความให้อยู่กลางแนวนอน */
}

.toggle-button.active {
  background-color: #56A7F5;
  color: white;
}

.toggle-button:hover {
  opacity: 0.9;
}

.toggle-button:not(.active) {
  background-color: #ECECEC; /* สีเทาเมื่อไม่ได้เลือก */
  color: #56A7F5; /* ตัวหนังสือสีฟ้า */
}
</style>  