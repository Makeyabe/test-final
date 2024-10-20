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

        <!-- Iframe สำหรับแสดงแผนที่ -->
        <iframe
          v-if="mapUrl"
          :src="mapUrl"
          allowfullscreen=""
          loading="lazy"
          referrerpolicy="no-referrer-when-downgrade"
          class="map-iframe">
        </iframe>
      </div>
    </div>
  </div>
</template>

<script setup>
import SidebarTeacher from '/pages/components/SidebarTeacher.vue';
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

// สร้างตัวแปรเพื่อเก็บ URL แผนที่
const mapUrl = ref('');

// ฟังก์ชันในการตั้งค่า mapUrl จาก localStorage
const setMapUrl = (url) => {
  localStorage.setItem('mapUrl', url);
  mapUrl.value = url;
};

// ฟังก์ชันในการดึง mapUrl จาก localStorage
const getMapUrl = () => {
  const storedUrl = localStorage.getItem('mapUrl');
  if (storedUrl) {
    mapUrl.value = storedUrl;
  } else {
    // หากไม่มี URL ให้กำหนดเป็นค่าเริ่มต้น
    const defaultUrl = 'https://www.google.com/maps/place/Demonstration+School+of+Khon+Kaen+University+Nong+Khai+Campus/@17.8064835,102.7459776,16z/data=!4m14!1m7!3m6!1s0x31239d708023aa15:0x50c037631d1d7650!2sFountain+Roundabout!8m2!3d17.4043878!4d102.7939425!16s%2Fg%2F11cncb3frf!3m5!1s0x31247d2f92e78029:0x432ba858802e1735!8m2!3d17.8105875!4d102.7439352!16s%2Fg%2F11h0333y21?entry=ttu&g_ep=EgoyMDI0MTAwNS4yIKXMDSoASAFQAw%3D%3D';
    setMapUrl(defaultUrl);
  }
};

// เรียกใช้ฟังก์ชัน getMapUrl เมื่อคอมโพเนนต์โหลด
onMounted(() => {
  getMapUrl();
});

// การจัดการ router
const router = useRouter();
const isActive = ref('map');
const goToMap = () => {
  isActive.value = 'map';
  router.push('/teacher/MapStudents');
};

const goToProfile = () => {
  isActive.value = 'info';
  router.push('/teacher/Info-Students');
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

    /* Container for input and button, centered at the top */
  .map-url-container {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    margin: 20px auto; /* Center the container horizontally */
    max-width: 600px; /* Optional: Limit the width */
  }

  /* Style for the Google Map URL input */
  .map-url-container input {
    flex: 1;
    max-width: 600px; /* Limit the max-width */
    padding: 10px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  /* Style for the Update button */
  .map-url-container button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #66b2f9;
    color: #fff;
    border: none;
    border-radius: 10px;
    transition: background-color 0.3s;
  }

  .map-url-container button:hover {
    background-color: #62acf2;
  }

  .map-url-container button:active {
    background-color: #62acf2;
  }
  /* Style for the iframe map display */
  iframe {
    width: 100%;
    max-width: 1050px;
    height: 465px;
    border: none;
    border-radius: 10px;
    margin-top: 20px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    display: block;
    margin-left: auto;
    margin-right: auto;
  }
    iframe.full-screen {
    height: 525px; /* ปรับความสูงให้เต็มที่เมื่อไม่ได้แก้ไข */
    }
</style>