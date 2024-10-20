<template>
  <Head>
      <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
  </Head>
  <div class="wrapper">
    <!-- ส่วนปฏิทิน -->
    <div class="datepicker-wrapper">
      <div class="datepicker-container">
        <div class="calendar-header">
          <button @click="prevMonth">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-left"><polyline points="15 18 9 12 15 6"></polyline></svg>
          </button>
          <span>{{ monthNames[month] }} {{ year }}</span>
          <button @click="nextMonth">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-right"><polyline points="9 18 15 12 9 6"></polyline></svg>
          </button>
        </div>
        <div class="calendar-body">
          <div class="calendar-day" v-for="day in weekDays" :key="day">{{ day }}</div>
          <div
            class="calendar-date"
            v-for="date in dates"
            :key="date.date"
            :class="[isWeekend(date) ? 'weekend' : '', date.bookingCount === 3 ? 'fully-booked' : (date.bookingCount === 2 ? 'almost-full' : 'available'), { selected: isSelected(date), disabled: !date.available }]"
            @click="selectDate(date)"
          >
            {{ date.date }}
            <small v-if="date.bookingCount > 0">({{ date.bookingCount }} คิว)</small>
          </div>
        </div>
        <div class="date-time-picker">
          <div class="date-picker">
            <input type="date" :value="formattedDate" readonly />
          </div>
          <div class="time-picker">
            <select v-model="selectedHour">
              <option v-for="h in hours" :key="h" :value="h">{{ h }}</option>
            </select>
            <span>:</span>
            <select v-model="selectedMinute">
              <option v-for="m in minutes" :key="m" :value="m">{{ m }}</option>
            </select>
          </div>
        </div>

        <!-- ส่วนฟอร์ม -->
        <div class="form-row">
          <div class="form-group">
            <label for="Name">ชื่อ-นามสกุล</label>
            <input type="text" id="Name" v-model="Name" />
          </div>
          <div class="form-group">
            <label for="stuId">รหัสประจำตัว</label>
            <input type="text" id="stuId" v-model="stuId" />
          </div>
        </div>
        <div class="form-button">
          <button class="book-button" @click="bookDate">จองวัน</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const Name = ref('');
const stuId = ref('');

function bookDate() {
  if (!Name.value || !stuId.value) {
    alert("กรุณากรอกข้อมูลให้ครบถ้วน");
  } else {
    alert(`จองวันที่สำหรับ ${Name.value} รหัสประจำตัว: ${stuId.value}`);
  }
}

const weekDays = ['อา', 'จ', 'อ', 'พ', 'พฤ', 'ศ', 'ส'];
const monthNames = ['มกราคม', 'กุมภาพันธ์', 'มีนาคม', 'เมษายน', 'พฤษภาคม', 'มิถุนายน', 'กรกฎาคม', 'สิงหาคม', 'กันยายน', 'ตุลาคม', 'พฤศจิกายน', 'ธันวาคม'];

const today = new Date();

const selectedDate = ref(null);
const year = ref(today.getFullYear());
const month = ref(today.getMonth());

const selectedHour = ref('13');
const selectedMinute = ref('00');

const userHasBooked = ref(false);
const previouslySelectedDate = ref(null);

const hours = Array.from({ length: 24 }, (_, i) => String(i).padStart(2, '0'));
const minutes = ['00', '05', '10' ,'15' ,'20' ,'25' , '30', '35', '40', '45', '50', '55'];

const formattedDate = ref(''); // ตั้งค่าเป็น ref

const dates = computed(() => {
  const startOfMonth = new Date(year.value, month.value, 1);
  const endOfMonth = new Date(year.value, month.value + 1, 0);
  const dates = [];
  
  for (let i = 0; i < startOfMonth.getDay(); i++) {
    dates.push({ date: '', available: false, bookingCount: 0 });
  }

  for (let i = 1; i <= endOfMonth.getDate(); i++) {
    const currentDate = new Date(year.value, month.value, i);
    const dayOfWeek = currentDate.getDay();
    const isWeekend = dayOfWeek === 0 || dayOfWeek === 6;

    dates.push({
      date: i,
      available: currentDate >= today && !isWeekend,
      bookingCount: 0,
    });
  }

  return dates;
});

function selectDate(date) {
  if (userHasBooked.value) {
    cancelPreviousBooking();
  }

  if (date.available && date.bookingCount < 3) {
    selectedDate.value = {
      date: date.date,
      month: month.value,
      year: year.value
    };

    // อัปเดต formattedDate ให้ตรงกับ selectedDate
    formattedDate.value = new Date(year.value, month.value, date.date + 1).toISOString().split('T')[0]; // เพิ่ม 1 เพื่อให้ตรงกับวันที่ที่ถูกเลือก

    date.bookingCount++;
    userHasBooked.value = true;
    previouslySelectedDate.value = date;
  } else {
    alert("ไม่สามารถจองคิวได้ คิวเต็มแล้ว");
  }
}

function cancelPreviousBooking() {
  if (previouslySelectedDate.value) {
    previouslySelectedDate.value.bookingCount--;
    previouslySelectedDate.value = null;
  }
}

function prevMonth() {
  if (month.value === 0) {
    month.value = 11;
    year.value--;
  } else {
    month.value--;
  }
}

function nextMonth() {
  if (month.value === 11) {
    month.value = 0;
    year.value++;
  } else {
    month.value++;
  }
}

function isSelected(date) {
  return selectedDate.value && selectedDate.value.date === date.date && date.available;
}

function isWeekend(date) {
  const currentDate = new Date(year.value, month.value, date.date);
  return currentDate.getDay() === 0 || currentDate.getDay() === 6;
}
</script>

<style scoped>
/* กำหนดให้ทุกอย่างใช้ฟอนต์ Inter */
* {
  font-family: 'Inter', sans-serif;
  box-sizing: border-box; /* เพื่อให้การจัดการขนาดขององค์ประกอบถูกต้อง */
}

/* ตั้งค่าสำหรับ input type="date" */
input[type="date"] {
  font-family: 'Inter', sans-serif;
}

/* ตั้งค่าส่วน wrapper และองค์ประกอบอื่น ๆ */
.wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.datepicker-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: -130px;
}

.datepicker-container {
  display: inline-block;
  padding: 20px;
  border-radius: 12px;
  background-color: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  width: 433.6px; /* ปรับขนาดเพิ่มขึ้น 3% จาก 420px */
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  font-weight: bold;
  font-size: 18px;
}

.calendar-header button {
  background: none;
  border: none;
  padding: 10px;
  cursor: pointer;
  transition: color 0.2s ease;
}

.calendar-header button:hover {
  color: #56A7F5;
}

.calendar-header button svg {
  stroke: currentColor;
  width: 24px;
  height: 24px;
}

.calendar-header span {
  font-size: 18px;
  font-weight: 500;
  color: #333;
  margin: 0 15px;
}

.calendar-body {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 8px;
  padding: 10px 0;
}

.calendar-day,
.calendar-date {
  text-align: center;
  font-size: 14px;
  padding: 8px;
}

.calendar-day {
  font-weight: bold;
  color: #555;
}

.calendar-date {
  cursor: pointer;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: background-color 0.3s, color 0.3s;
}

.calendar-date.available {
  color: green;
}

.calendar-date.almost-full {
  color: orange;
}

.calendar-date.fully-booked {
  color: red;
}

.calendar-date.disabled {
  color: #ccc;
  pointer-events: none;
}

.calendar-date.weekend {
  color: red;
  pointer-events: none;
}

.calendar-date:hover:not(.disabled):not(.weekend) {
  background-color: #ededed;
}

.date-time-picker {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-top: 1px solid #eee;
  margin-top: 10px;
}

.date-picker {
  flex-grow: 1;
  margin-right: 20px;
}

.time-picker {
  display: flex;
  align-items: center;
}

.date-time-picker input[type="date"] {
  font-size: 14px;
  padding: 6px;
  border-radius: 6px;
  border: 1px solid #ccc;
  background-color: #f9f9f9;
  width: 120px; /* ปรับความกว้างเป็น 120px */
  text-align: center; /* จัดข้อความให้อยู่ตรงกลาง */
}

.time-picker select {
  font-size: 14px;
  padding: 6px;
  border-radius: 6px;
  border: 1px solid #ccc;
  background-color: #f9f9f9;
  width: 60px;
  text-align: center;
  margin-right: 5px;
}

.time-picker span {
  font-size: 16px;
  padding: 0 5px;
}

.form-row {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
  width: 100%;
  max-width: 400px;
  gap: 10px; /* เพิ่มระยะห่างระหว่างช่องกรอกข้อมูล */
  color: #5B5B5B;
}

.form-group {
  display: flex;
  flex-direction: column;
  width: 100%; /* ให้ฟอร์มเต็มพื้นที่ */
}

.form-group label {
  font-weight: normal; /* ทำให้ข้อความใน label ไม่เป็นตัวหนา */
  margin-bottom: 5px;
  color: #5B5B5B; /* ปรับสีของข้อความ */
}

.form-group input {
  padding: 10px;
  font-size: 14px;
  font-weight: normal; /* ทำให้ข้อความใน input ไม่เป็นตัวหนา */
  border-radius: 6px;
  border: 1px solid #ccc;
  width: 100%;
}

.form-button {
  display: flex;
  justify-content: center;
  margin-top: 10px;
}

.book-button {
  background-color: #56A7F5;
  color: white;
  padding: 10px 20px;
  margin-top: 10px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  width: 100px; /* ให้ปุ่มมีขนาดความกว้างคงที่ */
}

.book-button:hover {
  background-color: #5ca3e6;
}
</style>
