<template>
  <Head>
    <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet" />
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet" />
  </Head>
  <div class="container">
    <Sidebar />
    <div class="main-content">
      <div class="header">
        <h1>HOME ENGAGE</h1>
      </div>
      <div class="content-area">
        <div class="info-box">
          <span class="info-text">{{ user.Name }}</span>
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
        <StudentDetails :isEditing="isEditing" :onToggleEdit="toggleEdit" @updateData="handleUpdateData" />
      </div>
    </div>
  </div>
</template>

<script setup>

import { ref } from 'vue';
import { useRouter } from 'vue-router';
import StudentDetails from '/pages/components/StudentDetails.vue';
import Sidebar from '/pages/components/Sidebar.vue';

const user = ref({ Name: "" });
const isEditing = ref(false);
const isActive = ref('info');

const toggleEdit = () => {
  isEditing.value = !isEditing.value;
  localStorage.setItem('isEditing', JSON.stringify(isEditing.value));
};

const router = useRouter();

const goToMap = () => {
  isActive.value = 'map';
  router.push('/student/Map-Student');
};

const goToProfile = () => {
  isActive.value = 'info';
  router.push('/student/Profile-Student');
};

onMounted(() => {
  const savedEditState = localStorage.getItem('isEditing');
  if (savedEditState) {
    isEditing.value = JSON.parse(savedEditState);
  }
});

const handleUpdateData = async (updateData) => {
  user.value.Name = await updateData 
}

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
  background-color: #ECECEC;
  border-radius: 8px 8px 0 0;
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
  background-color: #ff6b6b;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 10px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.edit-button:hover {
  background-color: #ff5252;
}

.save-button {
  background-color: #55c058;
  color: white;
}

.save-button:hover {
  background-color: #69b76b;
}

.button-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding-top: 20px;
  width: 100%;
}

.button-group {
  display: flex;
  gap: 30px;
}

.toggle-button {
  text-decoration: none;
  display: inline-block;
  width: 120px;
  height: 40px;
  line-height: 40px;
  border: none;
  border-radius: 15px;
  font-size: 18px;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
  outline: none;
  color: #56A7F5;
  background-color: #ECECEC;
  text-align: center;
}

.toggle-button.active {
  background-color: #56A7F5;
  color: white;
}

.toggle-button:hover {
  opacity: 0.9;
}

.toggle-button:not(.active) {
  background-color: #ECECEC;
  color: #56A7F5;
}
</style>
