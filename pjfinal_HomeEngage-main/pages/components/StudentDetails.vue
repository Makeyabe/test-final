<template>
    <Head>
      <link href="https://fonts.googleapis.com/css2?family=Chewy&display=swap" rel="stylesheet">
      <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    </Head>
    <div v-if="isLoading">
        ...loading
      </div>
    <div class="student-details" v-else>
      

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล</label>
                        <span class="detail-span">{{ formData.Name }}</span>
                    </div>
                    <div class="field">
                        <label>ชื่อเล่น</label>
                        <span class="detail-span">{{ formData.Nickname }}</span>
                    </div>
                    <div class="field">
                        <label>รหัสประจำตัว</label>
                        <span class="detail-span">{{ formData.StuId }}</span>
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span">{{ formData.StuPhone }}</span>
                    </div>
                    <div class="field">
                        <label>ระดับชั้น</label>
                        <span class="detail-span">{{ formData.StuClass }}</span>
                    </div>
                    <div class="field">
                        <label>เกิดวันที่</label>
                        <span class="detail-span">{{ formData.StuBirthDate }}</span>
                    </div>
                </div>

                <div class="row">
                    <div class="field full-width">
                        <label>ที่อยู่</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Address }}</span>
                        <input v-if="isEditing" v-model="formData.Address" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระยะทางจากบ้านมาโรงเรียน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Distance }}</span>
                        <input v-if="isEditing" v-model="formData.Distance" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เดินทางมาโรงเรียนโดย</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Transport }}</span>
                        <input v-if="isEditing" v-model="formData.Transport" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ความสามารถพิเศษ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Skills }}</span>
                        <input v-if="isEditing" v-model="formData.Skills" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (บิดา)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.FatherName }}</span>
                        <input v-if="isEditing" v-model="formData.FatherName" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>อาชีพ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.FatherJob }}</span>
                        <input v-if="isEditing" v-model="formData.FatherJob" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.FatherPhone }}</span>
                        <input v-if="isEditing" v-model="formData.FatherPhone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เงินเดือน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.FatherSalary }}</span>
                        <input v-if="isEditing" v-model="formData.FatherSalary" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระดับการศึกษา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.FatherEdu }}</span>
                        <input v-if="isEditing" v-model="formData.FatherEdu" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (มารดา)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.MotherName }}</span>
                        <input v-if="isEditing" v-model="formData.MotherName" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>อาชีพ</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.MotherJob }}</span>
                        <input v-if="isEditing" v-model="formData.MotherJob" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.MotherPhone }}</span>
                        <input v-if="isEditing" v-model="formData.MotherPhone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เงินเดือน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.MotherSalary }}</span>
                        <input v-if="isEditing" v-model="formData.MotherSalary" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ระดับการศึกษา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.MotherEdu }}</span>
                        <input v-if="isEditing" v-model="formData.MotherEdu" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ชื่อ-นามสกุล (ผู้ปกครอง)</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.ParentName }}</span>
                        <input v-if="isEditing" v-model="formData.ParentName" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เกี่ยวข้องเป็น</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.Relation }}</span>
                        <input v-if="isEditing" v-model="formData.Relation" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>เบอร์โทรศัพท์</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.ParentPhone }}</span>
                        <input v-if="isEditing" v-model="formData.ParentPhone" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field full-width">
                        <label>ที่อยู่ที่ติดต่อได้</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.ParentAddress }}</span>
                        <input v-if="isEditing" v-model="formData.ParentAddress" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>สถานภาพของบิดามารดา</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.PStatus }}</span>
                        <input v-if="isEditing" v-model="formData.PStatus" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>นักเรียนพักอาศัยอยู่กับใคร</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.LivesWith }}</span>
                        <input v-if="isEditing" v-model="formData.LivesWith" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ครอบครัวนักเรียนมีสมาชิกทั้งหมดกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.FamCount }}</span>
                        <input v-if="isEditing" v-model="formData.FamCount" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ปัจจุบันมีพี่น้องกำลังศึกษาอยู่กี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.SibStudy }}</span>
                        <input v-if="isEditing" v-model="formData.SibStudy" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>

                <div class="row">
                    <div class="field">
                        <label>ประกอบอาชีพกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.EmpCount }}</span>
                        <input v-if="isEditing" v-model="formData.EmpCount" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                    <div class="field">
                        <label>ไม่ประกอบอาชีพกี่คน</label>
                        <span class="detail-span" v-if="!isEditing">{{ formData.UnempCount }}</span>
                        <input v-if="isEditing" v-model="formData.UnempCount" class="Editdetail-span" @blur="stopEditing"  />
                    </div>
                </div>
    </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  props: {
    isEditing: {
      type: Boolean,
      required: true,
    },
    onToggleEdit: {
      type: Function,
      required: true,
    }
  },
  emits: ['updateData'],  // Correctly declare the emitted event
  setup(props, { emit }) {
    const formData = ref({
      Name: "",
      Nickname: "",
      StuId: "",
      StuPhone: "",
      StuClass: "",
      StuBirthDate: "",
      Address: "",
      Distance: "",
      Transport: "",
      Skills: "",
      FatherName: "",
      FatherJob: "",
      FatherPhone: "",
      FatherSalary: "",
      FatherEdu: "",
      MotherName: "",
      MotherJob: "",
      MotherPhone: "",
      MotherSalary: "",
      MotherEdu: "",
      ParentName: "",
      Relation: "",
      ParentPhone: "",
      ParentAddress: "",
      PStatus: "",
      LivesWith: "",
      FamCount: "",
      SibStudy: "",
      EmpCount: "",
      UnempCount: ""
    });

    const isLoading = ref(true);
    const hasError = ref(false);
    const errorMessage = ref('');

    onMounted(async () => {
      try {
        const username = localStorage.getItem('username');

        if (username) {
          const response = await axios.get(`http://localhost:8000/api/student/${username}`);
          if (response && response.data) {
            formData.value = response.data;  // Assign the fetched data
            await emit('updateData', response.data.Name);  // Emit the data to parent
          } else {
            throw new Error("Invalid student data");
          }
        } else {
          throw new Error("No username found in localStorage");
        }
      } catch (error) {
        console.error("Error fetching student data:", error.message);
        hasError.value = true;
        errorMessage.value = error.message || "An error occurred while fetching student data.";
      } finally {
        isLoading.value = false;
      }
    });

    return {
      formData,
      isLoading,
      hasError,
      errorMessage,
    };
  }
};
</script>

<style scoped>
.student-details {
    padding: 40px;
  }

  .row {
    display: flex;
    margin-bottom: 5px;
    justify-content: space-between; /* กระจาย item ให้มีพื้นที่เท่ากัน */
  }

  .field {
     flex: 1;
     margin: 5px;
  }
    
  .full-width {
    flex: 2;
  }

  label {
    font-family: 'Inter', sans-serif;
    font-weight: 400;
    font-size: 14px;
    margin-bottom: 5px;
    display: block;
    color: #5B5B5B;
  }

  .detail-span {
    background-color: #E1E1E1; /* สีพื้นหลัง */
    color: #5f5f5f; /* สีตัวหนังสือ */
    font-family: 'Inter', sans-serif;
    font-weight: 400;
    font-size: 14px; /* ขนาดตัวหนังสือ */
    padding: 8px; /* เพิ่ม padding */
    border-radius: 4px; /* มุมมน */
    display: block;
}
.Editdetail-span {
  width: 100%; /* ทำให้ input มีขนาดเท่ากับ container */
  padding: 5px; /* กำหนด padding เพื่อให้ input ดูเหมาะสม */
  background-color: #F9F9F9; /* สีพื้นหลัง */
  color: #3c3c3c; /* สีตัวหนังสือ */
  font-family: 'Inter', sans-serif;
  font-weight: 400;
  font-size: 14px; /* ขนาดตัวหนังสือ */
  padding: 8px; /* เพิ่ม padding */
  border-radius: 4px; /* มุมมน */
  border: 1px solid #3c3c3c; /* กำหนดเส้นขอบของ input */
  border-radius: 4px; /* มุมมน */
  box-sizing: border-box; /* ทำให้ padding ไม่ทำให้ขนาดของ input ใหญ่ขึ้น */
}
.Editdetail-span, .detail-span {
  transition: all 0.3s ease-in-out; /* เพิ่มการเปลี่ยนแปลงที่นุ่มนวล */
}
</style>