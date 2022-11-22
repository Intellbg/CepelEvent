<script setup lang="ts">
import { ref } from 'vue';
interface MonthlyConsumption {
    energy: number;
    diesel: number;
    production: number;
}
const data = ref<MonthlyConsumption[]>(
    [
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
        { energy: 0, diesel: 0, production: 0 },
    ]
)
const months = [
    "Ene", "Feb", "Mar", "Abr", "May", "Jun", "Jul", "Ago", "Sep", "Oct", "Nov", "Dic"
]
const props=defineProps({
    year: Number,
})
</script>

<template>
    <h3 class="title is-4">{{ props.year }}</h3>
    <table class="table">
        <thead>
            <tr>
                <th>Mes</th>
                <th>E. Eléctrica [kW/h]</th>
                <th>Diesel [gal]</th>
                <th>Producción [kg]</th>
            </tr>
        </thead>
        <tr v-for="(row, index) in data">
            <td>{{ months[index] }}</td>
            <td v-for="col in row">
                <div class="control"><input class="input" type="number" required></div>
            </td>
        </tr>
    </table>
</template>

<style scoped>
input {
    width: 80%;
}

td {
    text-align: center;
}

.load_csv {
    color: red;
}
</style>
