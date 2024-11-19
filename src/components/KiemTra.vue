<script setup>
import { ref } from "vue";

let nhanViens = ref([
  {
    id: 1,
    name: "employee 1",
    email: "email 1",
    phone: "phone 1",
  },
  {
    id: 2,
    name: "employee 2",
    email: "email 2",
    phone: "phone 2",
  },
  {
    id: 3,
    name: "employee 3",
    email: "email 3",
    phone: "phone 3",
  },
  {
    id: 4,
    name: "employee 4",
    email: "email 4",
    phone: "phone 4",
  },
  {
    id: 5,
    name: "employee 5",
    email: "email 5",
    phone: "phone 5",
  },
]);

const modelStudent = ref({
  id: "",
  name: "",
  email: "",
  phone: "",
});

const isEditing = ref(false);

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
</script>

<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Quản Lý Nhân Viên</h2>
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
        <tr v-for="nv in nhanViens" :key="nv.id">
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
  </div>
</template>

<style scoped></style>
