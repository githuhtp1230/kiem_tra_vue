<script setup>
import { ref, computed } from "vue";

let nhanViens = ref([
  { id: 1, name: "employee 1", email: "email 1", phone: "phone 1" },
  { id: 2, name: "employee 2", email: "email 2", phone: "phone 2" },
  { id: 3, name: "employee 3", email: "email 3", phone: "phone 3" },
  { id: 4, name: "employee 4", email: "email 4", phone: "phone 4" },
  { id: 5, name: "employee 5", email: "email 5", phone: "phone 5" },
  { id: 6, name: "employee 6", email: "email 6", phone: "phone 6" },
  { id: 7, name: "employee 7", email: "email 7", phone: "phone 7" },
  { id: 8, name: "employee 8", email: "email 8", phone: "phone 8" },
  { id: 9, name: "employee 9", email: "email 9", phone: "phone 9" },
  { id: 10, name: "employee 10", email: "email 10", phone: "phone 10" },
]);

const modelStudent = ref({
  id: "",
  name: "",
  email: "",
  phone: "",
});

const isEditing = ref(false);
let searchQuery = ref("");
const currentPage = ref(1);
const itemsPerPage = ref(5); // Số lượng nhân viên hiển thị mỗi trang

const add = () => {
  nhanViens.value.unshift({ ...modelStudent.value });
  resetForm();
};

const edit = (nhanVien) => {
  modelStudent.value = { ...nhanVien };
  isEditing.value = true;
};

const complete = () => {
  const index = nhanViens.value.findIndex(
    (nv) => nv.id === modelStudent.value.id
  );
  if (index !== -1) {
    nhanViens.value[index] = { ...modelStudent.value };
  }
  resetForm();
};

const deleteEmployee = (id) => {
  const index = nhanViens.value.findIndex((nhanVien) => nhanVien.id === id);
  if (index !== -1) {
    nhanViens.value.splice(index, 1);
  }
};

const resetForm = () => {
  modelStudent.value = {
    id: "",
    name: "",
    email: "",
    phone: "",
  };
  isEditing.value = false;
};

// Lọc danh sách nhân viên dựa trên từ khóa tìm kiếm
const filteredNhanViens = computed(() => {
  return nhanViens.value.filter((nv) => {
    return (
      nv.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      nv.email.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      nv.phone.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  });
});

// Lấy danh sách nhân viên của trang hiện tại
const paginatedNhanViens = computed(() => {
  const startIndex = (currentPage.value - 1) * itemsPerPage.value;
  const endIndex = startIndex + itemsPerPage.value;
  return filteredNhanViens.value.slice(startIndex, endIndex);
});

// Tính tổng số trang
const totalPages = computed(() => {
  return Math.ceil(filteredNhanViens.value.length / itemsPerPage.value);
});

// Chuyển đến trang tiếp theo
const nextPage = () => {
  if (currentPage.value < totalPages.value) {
    currentPage.value++;
  }
};

// Chuyển đến trang trước
const prevPage = () => {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
};
</script>

<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Quản Lý Nhân Viên</h2>

    <!-- Tìm kiếm -->
    <div class="mb-4">
      <input
        type="text"
        class="form-control"
        placeholder="Tìm kiếm nhân viên"
        v-model="searchQuery"
      />
    </div>

    <div class="card p-4 mb-4 shadow">
      <h5 class="mb-3">Thêm hoặc Sửa Nhân Viên</h5>
      <form @submit.prevent="isEditing ? complete() : add()">
        <div class="row g-3">
          <div class="col-md-3">
            <label for="employeeId" class="form-label">ID</label>
            <input
              type="text"
              class="form-control"
              id="employeeId"
              v-model="modelStudent.id"
              placeholder="Nhập ID"
              required
              :readonly="isEditing"
            />
          </div>
          <div class="col-md-3">
            <label for="employeeName" class="form-label">Tên</label>
            <input
              type="text"
              class="form-control"
              id="employeeName"
              v-model="modelStudent.name"
              placeholder="Nhập tên"
              required
            />
          </div>
          <div class="col-md-3">
            <label for="employeeEmail" class="form-label">Email</label>
            <input
              type="text"
              class="form-control"
              id="employeeEmail"
              v-model="modelStudent.email"
              placeholder="Nhập email"
              required
            />
          </div>
          <div class="col-md-3">
            <label for="employeePhone" class="form-label">Số điện thoại</label>
            <input
              type="tel"
              class="form-control"
              id="employeePhone"
              v-model="modelStudent.phone"
              placeholder="Nhập số điện thoại"
              required
            />
          </div>
        </div>
        <div class="mt-4 d-flex justify-content-between">
          <button type="submit" class="btn btn-primary w-100 me-2">
            {{ isEditing ? "Cập nhật Nhân Viên" : "Lưu Nhân Viên" }}
          </button>
          <button
            v-if="isEditing"
            class="btn btn-secondary w-100"
            @click="resetForm"
          >
            Hủy
          </button>
        </div>
      </form>
    </div>

    <!-- Hiển thị bảng nhân viên -->
    <table class="table table-striped table-hover">
      <thead class="table-dark">
        <tr>
          <th>ID</th>
          <th>Tên</th>
          <th>Email</th>
          <th>Số điện thoại</th>
          <th class="text-center">Hành Động</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="nv in paginatedNhanViens" :key="nv.id">
          <td>{{ nv.id }}</td>
          <td>{{ nv.name }}</td>
          <td>{{ nv.email }}</td>
          <td>{{ nv.phone }}</td>
          <td class="text-center">
            <button @click="edit(nv)" class="btn btn-warning btn-sm me-2">
              Sửa
            </button>
            <button
              @click="deleteEmployee(nv.id)"
              class="btn btn-danger btn-sm"
            >
              Xóa
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Phân trang -->
    <div class="d-flex justify-content-between">
      <button
        class="btn btn-secondary"
        @click="prevPage"
        :disabled="currentPage === 1"
      >
        Trước
      </button>
      <span>Trang {{ currentPage }} / {{ totalPages }}</span>
      <button
        class="btn btn-secondary"
        @click="nextPage"
        :disabled="currentPage === totalPages"
      >
        Sau
      </button>
    </div>
  </div>
</template>

<style scoped></style>
