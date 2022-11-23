<script setup lang="ts">
import { ref, watch } from 'vue';

const months = [
    "Ene", "Feb", "Mar", "Abr", "May", "Jun", "Jul", "Ago", "Sep", "Oct", "Nov", "Dic"
]
const props = defineProps([
    'year',
    'info',
])

const data = ref<number[][] | string[][]>(props.info)

watch(data, props.info, { deep: true })
</script>
<template>
    <h3 class="title is-4">{{ props.year }}</h3>
    <table class="table">
        <thead>
            <tr>
                <th>
                    <p>Mes</p>
                </th>
                <th>
                    <p>E. Eléctrica [kW/h]</p>
                </th>
                <th>
                    <p>Diesel [gal]</p>
                </th>
                <th>
                    <p>Producción [kg]</p>
                </th>
            </tr>
        </thead>
        <tr v-for="(row, index) in data">
            <td>
                <p>{{ months[index] }}</p>
            </td>
            <td v-for="(col, colIndex) in row">
                <div class="control"><input class="input" v-model="data[index][colIndex]" type="number" min="0" step="0.01"
                        required></div>
            </td>
        </tr>

    </table>
</template>

<style scoped>
input {
    width: 100%;
}

.table {
    width: 100%;
}

td {
    text-align: center;
}

.load_csv {
    color: red;
}

p {
    text-align: center;
}

th {
    text-align: center;
}
</style>
