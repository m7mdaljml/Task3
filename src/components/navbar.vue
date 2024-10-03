<template>

    <div class="container-fluid" style="background-color: lightgray;padding:10px;">
        <div class="row mb-3">
            <div class="col-md-4">
                <a class="navbar-brand" href="https://cubesplatform.com/cubes-platform" target="_blank"><img
                        class="logo" :src="logo"></a>
            </div>
            <div class="col-md-4">
                <select class="form-select" @change="event => $emit('onSelect', JSON.parse(event.target.value))">
                    <option value='{"data":"Employees", "schema":"EmpSchema"}'>Employees</option>
                    <option value='{"data":"Cars", "schema":"CarSchema"}'>Cars</option>
                </select>
            </div>
            <div class="col-md-4">
                <input v-model="text" type="text" class="form-control" placeholder="Search record" @input="$emit('onInput',text)">
            </div>
            <!--<div class="col-md-3">
                <div>
                    <button class="btn btn-outline-primary m-1">Export</button>
                    <button class="btn btn-outline-secondary">Import</button>
                </div>
            </div>-->
        </div>

    </div>
</template>
<script setup>
import logo from '@/assets/logo.png'
import {ref} from 'vue'
const text = ref('')
const emit = defineEmits([
    'onSelect',
    'onInput'
])
</script>
<style scoped>
.col-md-3 {
    align-content: center;
}
</style>