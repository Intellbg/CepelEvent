<script setup lang="ts">
import { reactive, ref } from 'vue';
const emit = defineEmits(['nextInfo'])

const info = ref({
    name: "",
    email: "",
    company: "",
    ruc: "",
    website: "",
    address: "",
    area: "",
    phone: ""
})
const errors = ref({
    name: "",
    email: "",
    company: "",
    ruc: "",
    address: "",
    area: "",
    phone: ""
})

function validate() {
    for (let val in info.value) {
        let value = info.value[val as keyof typeof info.value]
        if (value == '' && val.toString() !== 'website') {
            errors.value[val as keyof typeof errors.value] = 'Información requerida'
            continue
        }
        errors.value[val as keyof typeof errors.value] = ''
    }
    errors.value.ruc = validateRuc(info.value.ruc.toString() || "")
    errors.value.phone = validatePhone(info.value.phone.toString() || "")
    for (let val in errors.value) {
        let value = errors.value[val as keyof typeof errors.value]
        if (value != '') {
            return
        }
    }
    emit('nextInfo', info.value)
}
function validateRuc(number: string) {
    if (!(/^\d+$/.test(number))) {
        return "Campo solo debe incluir números"
    }
    if (number.length !== 13) {
        return "Longitud de ruc incorrecta"
    }
    let provCode = parseInt(number.slice(0, 2))
    if (!((provCode <= 24) || provCode == 30)) {
        return "Código de provincia inválido"
    }
    let end = number.slice(10, 13)
    if (end !== '001') {
        return "Ultimos dígitos inválidos"
    }
    return ""
}
function validatePhone(number: string) {
    if (!(/^\d+$/.test(number))) {
        return "Campo solo debe incluir números"
    }
    if (number.length !== 10) {
        return "Longitud de teléfono incorrecta"
    }
    return ""
}
</script>

<template>
    <div class="field">
        <label class="label">Nombre Contacto</label>
        <div class="control">
            <input class="input" type="text" placeholder="Luis Aguilar" v-model="info.name" required>
        </div>
        <p class="help is-danger">{{ errors.name }}</p>

    </div>
    <div class="field">
        <label class="label">Email</label>
        <div class="control">
            <input class="input" type="email" placeholder="nombre@dominio.com" v-model="info.email" required>
        </div>
        <p class="help is-danger">{{ errors.email }}</p>
    </div>
    <div class="field">
        <label class="label">Nombre empresa</label>
        <div class="control">
            <input class="input" type="text" placeholder="Cepel" v-model="info.company" required>
        </div>
        <p class="help is-danger">{{ errors.company }}</p>
    </div>
    <div class="field">
        <label class="label">RUC</label>
        <div class="control">
            <input class="input" :class="{ 'is-danger': errors.ruc }" type="text" placeholder="1799999999001"
                v-model="info.ruc" required maxlength="13">
        </div>
        <p class="help is-danger">{{ errors.ruc }}</p>
    </div>
    <div class="field">
        <label class="label">Dirección</label>
        <div class="control">
            <input class="input" type="text" placeholder="Av. Americas 123" v-model="info.address" required>
        </div>
        <p class="help is-danger">{{ errors.address }}</p>
    </div>
    <div class="field">
        <label class="label">Teléfono</label>
        <div class="control">
            <input class="input" :class="{ 'is-danger': errors.phone }" type="text" placeholder="0999999999"
                v-model="info.phone" required maxlength="10">
        </div>
        <p class="help is-danger">{{ errors.phone }}</p>
    </div>
    <div class="field">
        <label class="label">Área</label>
        <div class="control">
            <input class="input" type="text" placeholder="Agricola" v-model="info.area" required>
        </div>
        <p class="help is-danger">{{ errors.area }}</p>
    </div>
    <div class="field">
        <label class="label">Sitio Web</label>
        <div class="control">
            <input class="input" type="text" placeholder="https://dominio.com" v-model="info.website">
        </div>
    </div>

    <div class="field is-grouped">
        <div class="control">
            <button class="button is-link green" type="submit" @click="validate()">Siguiente</button>
        </div>
    </div>


</template>

<style scoped>
input {
    width: 80%;
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
</style>
