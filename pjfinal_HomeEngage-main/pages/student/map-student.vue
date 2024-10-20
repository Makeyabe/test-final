<template>
  <Head>
    <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
  </Head>
  <div class="container">
    <Sidebar />

    <div class="main-content">
      <div class="header">
        <h1>HOME ENGAGE</h1>
      </div>
      <div class="content-area">
        <div class="info-box">
          <span class="info-text">{{ user.name }}</span>
          <button class="edit-button" :class="{ 'save-button': isEditing }" @click="toggleEdit">
            {{ isEditing ? 'บันทึก' : 'แก้ไข' }}
          </button>
        </div>
        <div class="button-container">
          <div class="button-group">
            <button class="toggle-button" :class="{ active: isActive === 'info' }" @click="goToProfile">ข้อมูล</button>
            <button class="toggle-button" :class="{ active: isActive === 'map' }" @click="goToMap">แผนที่บ้าน</button>
          </div>
        </div>
        <div v-if="isEditing" class="map-url-container">
          <input v-model="mapInput" placeholder="Google map URL" />
          <button @click="updateMapUrl">อัพเดตแผนที่</button>
        </div>

        <!-- Iframe สำหรับแสดงแผนที่ -->
        <iframe
          v-if="user.mapUrl"
          :src="user.mapUrl"
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
import Sidebar from '/pages/components/Sidebar.vue';
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';

// ข้อมูลของผู้ใช้งาน รวมถึง URL แผนที่
const user = {
  mapUrl: 'https://www.google.com/maps/place/Demonstration+School+of+Khon+Kaen+University+Nong+Khai+Campus/@17.8064835,102.7459776,16z/data=!4m14!1m7!3m6!1s0x31239d708023aa15:0x50c037631d1d7650!2sFountain+Roundabout!8m2!3d17.4043878!4d102.7939425!16s%2Fg%2F11cncb3frf!3m5!1s0x31247d2f92e78029:0x432ba858802e1735!8m2!3d17.8105875!4d102.7439352!16s%2Fg%2F11h0333y21?entry=ttu&g_ep=EgoyMDI0MTAwNS4yIKXMDSoASAFQAw%3D%3D' // URL ของ iframe
};

// การจัดการ router
const router = useRouter();
const isActive = ref('map'); 
const goToMap = () => {
  isActive.value = 'map'; 
  router.push('/student/Map-student'); 
};

const goToProfile = () => {
  isActive.value = 'info'; 
  router.push('/student/Profile-student'); 
};

const isEditing = ref(JSON.parse(localStorage.getItem('isEditing')) || false); 

const toggleEdit = () => {
  isEditing.value = !isEditing.value;
  localStorage.setItem('isEditing', JSON.stringify(isEditing.value)); 
};

const mapInput = ref('');

// ฟังก์ชันสำหรับดึง URL แผนที่จากฐานข้อมูล
const fetchMapUrl = async () => {
  const storedUrl = localStorage.getItem('mapUrl'); 
  if (storedUrl) {
    user.mapUrl = storedUrl; 
  }
};

const updateMapUrl = () => {
  const url = mapInput.value.trim();
  let lat, lng;

  // รูปแบบสำหรับการดึงพิกัด
  const patterns = [
    /@(-?\d+\.\d+),(-?\d+\.\d+)/,
    /place\/.*?@(-?\d+\.\d+)/,
    /search\/(-?\d+\.\d+),(-?\d+\.\d+)/,
    /maps\/@(-?\d+\.\d+),(-?\d+\.\d+)/,
    /maps\/\?q=(-?\d+\.\d+),(-?\d+\.\d+)/,
    /(-?\d{1,3})°(\d{1,2})'(\d{1,2}\.\d+)"([NS])\s+(-?\d{1,3})°(\d{1,2})'(\d{1,2}\.\d+)"([EW])/
  ];

  for (let pattern of patterns) {
    const match = url.match(pattern);
    if (match) {
      if (pattern.source.includes('DMS')) {
        lat = convertDMSToDecimal(match[1], match[2], match[3], match[4]);
        lng = convertDMSToDecimal(match[5], match[6], match[7], match[8]);
      } else {
        lat = match[1];
        lng = match[2];
      }
      break;
    }
  }

  if (!lat && url.includes('maps.app.goo.gl')) {
    const proceed = confirm('ลิงก์ที่คุณป้อนไม่มีพิกัดโดยตรง กด OK เพื่อเปิดลิงก์ในแท็บใหม่ แล้วคัดลอก URL เต็มจากแถบที่อยู่ของเบราว์เซอร์');
    if (proceed) {
      window.open(url, '_blank');
    }
    return;
  }
  console.log(lat,lng);
  
  if (lat && lng) {
    // สร้าง URL ที่ถูกต้องสำหรับ iframe
    user.mapUrl = `https://maps.google.com/maps?q=${lat},${lng}&output=embed`;
    localStorage.setItem('mapUrl', user.mapUrl); 
    mapInput.value = ''; 
  } else {
    alert('ไม่สามารถดึงพิกัดจาก URL ได้');
  }
};

// ฟังก์ชันแปลง DMS เป็น Decimal Degrees
const convertDMSToDecimal = (degrees, minutes, seconds, direction) => {
  let decimal = parseFloat(degrees) + parseFloat(minutes) / 60 + parseFloat(seconds) / 3600;
  if (direction === 'S' || direction === 'W') {
    decimal *= -1;
  }
  return decimal;
};

// ดึง URL แผนที่เมื่อ component ถูกสร้าง
onMounted(fetchMapUrl);
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

  .edit-button {
        background-color: #ff6b6b; /* Matches the edit button color */
        color: white;
        border: none;
        padding: 8px 16px;
        border-radius: 10px;
        cursor: pointer;
        transition: background-color 0.3s ease;
  }

  .edit-button:hover {
        background-color: #ff5252; /* Hover effect color */
  }

  /* ปุ่มบันทึก */
  .save-button {
    background-color: #55c058; /* สีพื้นฐานของปุ่มบันทึก */
    color: white;
  }

  /* สีเมื่อ hover ปุ่มบันทึก */
  .save-button:hover {
    background-color: #69b76b; /* สีเมื่อ hover */
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

.map-image {
width: 100%; /* ปรับขนาดตามที่ต้องการ */
height: auto;
}
</style>