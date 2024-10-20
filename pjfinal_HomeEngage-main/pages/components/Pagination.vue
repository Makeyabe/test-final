<template>
  <div class="pagination">
    <button @click="prevPage" :disabled="currentPage === 1">
      <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-left">
        <polyline points="15 18 9 12 15 6"></polyline>
      </svg>
    </button>

    <button 
      v-if="currentPage > 0" 
      @click="setPage(1)"
      :class="{ active: currentPage === 1 }"
    >
      1
    </button>

    <span v-if="showEllipsisLeft">...</span>

    <button 
      v-for="page in pagesToShow" 
      :key="page" 
      @click="setPage(page)" 
      :class="{ active: currentPage === page }"
    >
      {{ page }}
    </button>

    <span v-if="showEllipsisRight">...</span>

    <button 
      v-if="totalPages > 1" 
      @click="setPage(totalPages)"
      :class="{ active: currentPage === totalPages }"
    >
      {{ totalPages }}
    </button>

    <button @click="nextPage" :disabled="currentPage === totalPages">
      <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-chevron-right">
        <polyline points="9 18 15 12 9 6"></polyline>
      </svg>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      required: true
    },
    totalPages: {
      type: Number,
      required: true
    }
  },
  computed: {
    showEllipsisLeft() {
      return this.currentPage > 4; // แสดง ellipsis ถ้าหน้าเกิน 3
    },
    showEllipsisRight() {
      return this.currentPage < this.totalPages - 1; // แสดง ellipsis ถ้าหน้าไม่ใช่หน้าสุดท้าย 2 หน้า
    },
    pagesToShow() {
      const pages = [];
      const maxVisiblePages = 1; // จำนวนหน้าที่จะแสดง

      if (this.currentPage <= 2) {
        // ถ้าอยู่ที่หน้า 1 หรือ 2 ให้แสดงหมายเลขหน้า 2, 3
        for (let i = 2; i <= Math.min(this.totalPages, maxVisiblePages + 1); i++) {
          pages.push(i);
        }
      } else if (this.currentPage >= this.totalPages - 1) {
        // ถ้าอยู่ที่หน้าสุดท้าย ให้แสดงหมายเลขหน้าที่อยู่ก่อนหน้า
        for (let i = this.totalPages - maxVisiblePages; i < this.totalPages; i++) {
          if (i > 1) {
            pages.push(i);
          }
        }
      } else {
        // ถ้าอยู่กลาง ๆ จะมีการแสดง currentPage
        pages.push(this.currentPage - 1, this.currentPage, this.currentPage + 1);
      }

      return pages;
    }
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.setPage(this.currentPage - 1);
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.setPage(this.currentPage + 1);
      }
    },
    setPage(page) {
      this.$emit('set-page', page);
    }
  }
};
</script>

<style scoped>
.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
}

.pagination button, .pagination span {
  border: none;
  background-color: #F0F0F0;
  color: #56A7F5; /* Default text color */
  margin: 0 6px;
  cursor: pointer;
  border-radius: 12px;
  padding: 14px 20px; /* ปรับเพิ่ม padding */
  font-size: 14px; /* ขนาดตัวอักษร */
}

.pagination button.active {
  background-color: #56A7F5; /* Highlighted background for the active page */
  color: white; /* White text for active page */
}

.pagination button:disabled {
  cursor: not-allowed;
  background-color: #ECECEC;
  color: #B0B0B0;
}

.ellipsis {
  cursor: default; /* Ellipsis is not clickable */
  color: #b0b0b0; /* Color for ellipsis */
}

/* ปรับขนาดของ SVG ให้เหมาะสม */
.pagination svg {
  vertical-align: middle; /* จัดให้ SVG อยู่กลางปุ่ม */
  width: 18px; /* ปรับขนาดลูกศรให้เล็กลง */
  height: 18px; /* ปรับขนาดลูกศรให้เล็กลง */
}
</style>