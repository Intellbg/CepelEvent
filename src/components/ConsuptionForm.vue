<script setup lang="ts">
import ConsumptionInput from './ConsumptionInput.vue';
import Papa from 'papaparse';
import { onMounted, ref, watch } from 'vue';

const data2021 = ref<number[][] | string[][]>([
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""]
])
const data2022 = ref<number[][] | string[][]>([
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
])

const counter2021 = ref(1)
const counter2022 = ref(1000)
const sendtError = ref<string[]>([])
const emit = defineEmits(["getConsumptionData"])

function loadCsv(event: any) {
    Papa.parse(event.target.files[0], {
        header: false,
        skipEmptyLines: true,
        dynamicTyping: true,
        complete: function (results: any) {
            console.log(results.data)
            results.data = results.data.slice(1);
            data2021.value = results.data.slice(0, 12);
            data2022.value = results.data.slice(12, 22);
            counter2021.value++
            counter2022.value++
            console.log('done')
        }
    })
}
function validate() {
    let celerror: string[] = []
    data2021.value.forEach((row, index) => {
        row.forEach((col, index2) => {
            if ((typeof col === 'string' && col.length === 0) || col === null) {
                celerror.push(`Valor en celda: ${index + 1} ${index2 + 1} inválido año 2021`)
            }
        })
    })
    data2022.value.forEach((row, index) => {
        row.forEach((col, index2) => {
            if ((typeof col === 'string' && col.length === 0) || col === null) {
                celerror.push(`Valor en celda: ${index + 1} ${index2 + 1} inválido año 2022`)
            }
        })
    })
    if (celerror.length !== 0) {
        sendtError.value = celerror
        return
    }
    emit('getConsumptionData', data2021, data2022)
}
</script>

<template>
    <form enctype="multipart/form-data">
        <label for="file-upload" class="button load_csv is-pulled-right green">
            Cargar CSV
        </label>
        <input type="file" @change="loadCsv($event)" accept=".csv" id="file-upload">
        <a href="@/assets/produccion.csv" download>
            <label class="button load_csv is-pulled-right ">
                Descargar Ejemplo
            </label>
        </a>
    </form>
    <ConsumptionInput :year=2021 :info="data2021" :key="counter2021" />
    <ConsumptionInput :year=2022 :info="data2022" :key="counter2022" />
    <p class="has-text-danger" v-for="(es) in sendtError">{{ es }}</p>
    <div class="field is-grouped">
        <button class="button is-link green" type="submit" @click="validate()">Enviar</button>
    </div>
</template>

<style scoped>
input {
    width: 80%;
}

.load_csv {
    z-index: 100;
    background-color: #485fc7;
    color: white
}

.control {
    text-align: center;
    width: 100%;
}

td {
    text-align: center;
}

.green {
    background-color: green !important
}

input[type="file"] {
    display: none;
}
</style>
