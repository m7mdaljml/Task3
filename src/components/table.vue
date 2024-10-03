<template>
    <div class="container mt-4">


        <div class="table-responsive">
            <table class="table table-striped">
                <thead>
                    <tr>
                        <th v-for="(key, index) in schema" :key="index">
                            {{ key }}
                        </th>
                        <th colspan="2">Action</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in data.slice(first, end)" :key="item.id">
                        <td v-for="(key, index) in schema" :key="index">
                            {{ item[key] }}
                        </td>
                        <td>
                            <button class="btn btn-outline-secondary btn-sm me-2">Edit</button>
                            <button class="btn btn-outline-danger btn-sm">Delete</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>

        <footer>
            <ul class="pagination justify-content-center">
                <button type="button" class="btn btn-outline-primary p-2 m-1" @click="decrement()"> &larr;</button>
                <button type="button" class="btn btn-outline-primary p-2 m-1" @click="increment()"> &rarr;</button>
            </ul>
        </footer>
    </div>
</template>

<script setup lang="ts">
import { defineProps, ref } from 'vue';
import axios from 'axios';
defineProps({
    data: {
        type: Array,
        required: true
    },
    schema: {
        type: Array,
        required: true
    }
})

let first = ref(0)
let end = ref(10)

const increment = () => {
    if (end.value <= 990) {
        first.value += 10
        end.value += 10
    }
}
const decrement = () => {
    if (first.value >= 10) {
        first.value -= 10
        end.value -= 10
    }
}

</script>

<style scoped>
.table{
    border: 1px solid lightgray;
    border-radius: 95% 5% 96% 4% / 4% 94% 6% 96% ;
}

</style>
