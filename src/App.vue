<template>
  <v-navbar @onSelect="ChangeData" @onInput="SelectUser" />
  <v-table :data="SearchResult" :schema="schema" @DeleteUser="deleteResult" @EditUser="editResult" />

  <div v-if="Flag" class="modal-overlay">
    <div class="modal-content">
      <h3>Edit Record</h3>
      <form @submit.prevent="saveEdit">
        <div v-for="(value, key) in editedItem" :key="key">
          <label>{{ key }}:</label>
          <input v-model="editedItem[key]" :disabled="key === 'id'"/>
        </div>
        <button type="submit" class="btn btn-primary mt-3 p-2 m-1">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-floppy"
            viewBox="0 0 16 16">
            <path d="M11 2H9v3h2z" />
            <path
              d="M1.5 0h11.586a1.5 1.5 0 0 1 1.06.44l1.415 1.414A1.5 1.5 0 0 1 16 2.914V14.5a1.5 1.5 0 0 1-1.5 1.5h-13A1.5 1.5 0 0 1 0 14.5v-13A1.5 1.5 0 0 1 1.5 0M1 1.5v13a.5.5 0 0 0 .5.5H2v-4.5A1.5 1.5 0 0 1 3.5 9h9a1.5 1.5 0 0 1 1.5 1.5V15h.5a.5.5 0 0 0 .5-.5V2.914a.5.5 0 0 0-.146-.353l-1.415-1.415A.5.5 0 0 0 13.086 1H13v4.5A1.5 1.5 0 0 1 11.5 7h-7A1.5 1.5 0 0 1 3 5.5V1H1.5a.5.5 0 0 0-.5.5m3 4a.5.5 0 0 0 .5.5h7a.5.5 0 0 0 .5-.5V1H4zM3 15h10v-4.5a.5.5 0 0 0-.5-.5h-9a.5.5 0 0 0-.5.5z" />
          </svg>
        </button>
        <button type="button" class="btn btn-danger mt-3 p-2 m-1" @click="Flag = false">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x-square"
            viewBox="0 0 16 16">
            <path
              d="M14 1a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1H2a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1zM2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2z" />
            <path
              d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708" />
          </svg>
        </button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import VTable from '@/components/table.vue'
import VNavbar from '@/components/navbar.vue'
import axios from 'axios'

const Cars = ref([])
const Employees = ref([])
const CarSchema = ref(['id', 'car_make', 'car_model_year', 'car_vin'])
const EmpSchema = ref(['id', 'first_name', 'last_name', 'email', 'gender', 'ip_address'])
const data = ref([])
const schema = ref([])
const SearchValue = ref('')
const Flag = ref(false)
const editedItem = ref({})

const GetData = async () => {
  try {
    const CarRes = await axios.get('cars.json')
    Cars.value = CarRes.data
    const EmployeesRes = await axios.get('employees.json')
    Employees.value = EmployeesRes.data

    data.value = Employees.value
    schema.value = EmpSchema.value
  } catch (err) {
    console.log(err)
  }
}

const ChangeData = (obj) => {
  if (obj.data === 'Employees') {
    data.value = Employees.value
    schema.value = EmpSchema.value
  } else if (obj.data === 'Cars') {
    data.value = Cars.value
    schema.value = CarSchema.value
  }
}

const SelectUser = (text) => {
  SearchValue.value = text.toLowerCase();
}

const SearchResult = computed(() => {
  if (!SearchValue.value) {
    return data.value
  }
  return data.value.filter((item) =>
    Object.keys(item).some((key) => String(item[key]).toLowerCase().includes(SearchValue.value))
  )
})

const deleteResult = (id) => {
  data.value = data.value.filter((item) => item.id !== id)
}

const editResult = (id) => {
  const itemToEdit = data.value.find(item => item.id === id)
  if (itemToEdit) {
    editedItem.value = { ...itemToEdit }
    Flag.value = true
  }
}

const saveEdit = () => {
  const index = data.value.findIndex(item => item.id === editedItem.value.id)
  if (index !== -1) {
    data.value[index] = { ...editedItem.value }
  }
  Flag.value = false
}


onMounted(async () => {
  await GetData()
})
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  width: 400px;
}

input {
  display: block;
  margin-bottom: 10px;
  padding: 8px;
  width: 100%;
}
</style>
