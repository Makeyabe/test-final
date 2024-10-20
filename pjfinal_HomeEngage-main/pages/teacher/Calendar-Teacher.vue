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
      </div>
    </div>
  </div>
</template>

<script setup>
import SidebarTeacher from '/pages/components/SidebarTeacher.vue'
import { useRouter } from 'vue-router'

const router = useRouter()

const logout = () => {
  console.log("Logging out...");
  router.push('/Login-Teacher')
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
</style>  