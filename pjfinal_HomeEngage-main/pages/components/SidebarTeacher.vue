<template>
    <Head>
      <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
      <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    </Head>
    <div class="sidebar">
        <div class="profile-section">
          <div class="profile-image">
            <img :src="user.profileImage" alt="Profile Image" />
          </div>
          <p class="profile-name">{{ user.Name }}</p>
          <p class="profile-detail"> รหัสประจำตัว {{ user.teacherId }}</p>
        </div>
        <div class="menu">
          <router-link to="/teacher/Home-Teacher" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-home"><path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path><polyline points="9 22 9 12 15 12 15 22"></polyline></svg></i>
            <span>หน้าแรก</span>
          </router-link>
          <router-link to="/teacher/Calendar-Teacher" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-calendar"><rect x="3" y="4" width="18" height="18" rx="2" ry="2"></rect><line x1="16" y1="2" x2="16" y2="6"></line><line x1="8" y1="2" x2="8" y2="6"></line><line x1="3" y1="10" x2="21" y2="10"></line></svg></i>
            <span>จองวันเยี่ยมบ้าน</span>
          </router-link>
          <router-link to="/teacher/ProfileStudent" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="26" height="26" fill="currentColor" class="bi bi-journal-text" viewBox="0 0 17 17">
                <path d="M5 10.5a.5.5 0 0 1 .5-.5h2a.5.5 0 0 1 0 1h-2a.5.5 0 0 1-.5-.5m0-2a.5.5 0 0 1 .5-.5h5a.5.5 0 0 1 0 1h-5a.5.5 0 0 1-.5-.5m0-2a.5.5 0 0 1 .5-.5h5a.5.5 0 0 1 0 1h-5a.5.5 0 0 1-.5-.5m0-2a.5.5 0 0 1 .5-.5h5a.5.5 0 0 1 0 1h-5a.5.5 0 0 1-.5-.5"/>
                <path d="M3 0h10a2 2 0 0 1 2 2v12a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2v-1h1v1a1 1 0 0 0 1 1h10a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H3a1 1 0 0 0-1 1v1H1V2a2 2 0 0 1 2-2"/>
                <path d="M1 5v-.5a.5.5 0 0 1 1 0V5h.5a.5.5 0 0 1 0 1h-2a.5.5 0 0 1 0-1zm0 3v-.5a.5.5 0 0 1 1 0V8h.5a.5.5 0 0 1 0 1h-2a.5.5 0 0 1 0-1zm0 3v-.5a.5.5 0 0 1 1 0v.5h.5a.5.5 0 0 1 0 1h-2a.5.5 0 0 1 0-1z"/>
            </svg></i>
            <span>ข้อมูลนักเรียน</span>
          </router-link>
          <router-link to="/teacher/Conclude-Teacher" class="menu-item">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-bar-chart-2"><line x1="18" y1="20" x2="18" y2="10"></line><line x1="12" y1="20" x2="12" y2="4"></line><line x1="6" y1="20" x2="6" y2="14"></line></svg></i>
            <span>สรุปผล</span>
          </router-link>
        </div>
        <div class="logout-section">
          <button class="logout-button" @click="logout">
            <i><svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-log-out"><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"></path><polyline points="16 17 21 12 16 7"></polyline><line x1="21" y1="12" x2="9" y2="12"></line></svg></i>
            <span>Logout</span>
          </button>
        </div>
      </div>
</template>
  
<script setup>
  const user = {
    profileImage:'/images/User.png',
    Name: "นาย xxx xxx", 
    teacherId: "xxxxxxxxx-x"}
  
  const router = useRouter()
    const logout = () => {
    console.log("Logging out...");
    router.push('/')
  }
</script>
  
<style scoped>
  .sidebar {
  width: 280px;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  padding: 0;
  border-right: 1px solid #e0e0e0;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
  font-family: 'Inter', sans-serif;
    }

    .profile-section {
    text-align: center;
    padding: 30px 0;
    border-bottom: 1px solid #e0e0e0;
    }

    .profile-image img {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    background-color: #f0f0f0;
    object-fit: cover;
    }

    .profile-name {
    margin-top: 10px;
    font-size: 18px;
    font-weight: 500;
    color: #333;
    }

    .profile-detail {
    font-size: 14px;
    color: #888;
    margin-top: 5px;
    margin-bottom: 0;
    }

    .menu {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
    margin-top: 10px;
    }

    .menu-item {
    display: flex;
    align-items: center;
    padding: 12px 25px;
    font-size: 16px;
    color: #333;
    text-decoration: none;
    transition: background-color 0.3s ease, color 0.3s ease;
    }

    .menu-item i {
    margin-right: 15px;
    color: #56A7F5;
    font-size: 18px;
    }

    .menu-item span {
    flex: 1;
    }

    .menu-item:hover {
    background-color: #f9f9f9;
    color: #56A7F5;
    }

    .logout-section {
    border-top: 1px solid #e0e0e0;
    padding: 10px 0;
    text-align: center;
    }

    .logout-button {
    background-color: #ff6b6b;
    color: white;
    border: none;
    padding: 12px 25px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 25px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background-color 0.3s ease;
    width: 80%;
    margin: 0 auto;
    margin-top: 10px;
    }

    .logout-button i {
    margin-right: 10px;
    font-size: 16px;
    }

    .logout-button:hover {
    background-color: #ff5252;
    }
</style>
  