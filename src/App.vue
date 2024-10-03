<template>
  <v-navbar @onSelect="ChangeData" @onInput="SelectUser" />
  <v-table :data="SearchResult" :schema="schema" />
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
  SearchValue.value = text.toLowerCase()
}

const SearchResult = computed(() => {
  if (!SearchValue.value) {
    return data.value
  }
  return data.value.filter(item =>
    Object.keys(item).some(key =>
      String(item[key]).toLowerCase().includes(SearchValue.value)
    )
  )
})

onMounted(async () => {
  await GetData()
})
</script>

<style scoped></style>
