<script setup lang="ts">
import { reactive, ref } from 'vue';

interface Contact {
    name: String;
    email: String;
    company: String;
    ruc: String;
    website?: String;
    address: String;
    phone: String;
    area: String;
}
const info = ref<Contact>({
    name: "",
    email: "",
    company: "",
    ruc: "",
    website: "",
    address: "",
    area: "",
    phone: ""
})
const rawErrors = {
    name: "",
    email: "",
    company: "",
    ruc: "",
    address: "",
    area: "",
    phone: ""
}
const errors = reactive(rawErrors)
const emit = defineEmits(['nextInfo'])

function validate() {
    for (let val in info.value) {
        let value = info.value[val as keyof typeof info.value]
        if(value==''){
            errors[val as keyof typeof errors]='Información requerida'
            continue
        }
        errors[val as keyof typeof errors]=''
    }
    validateRuc(info.value.ruc.toString() || "")
    validatePhone(info.value.phone.toString() || "")
    if (errors !== rawErrors) {
        return
    }
    emit('nextInfo')
}

function validateRuc(number: string) {
    if (!(/^\d+$/.test(number))) {
        errors.ruc = "Campo solo debe incluir números"
        return
    }
    if (number.length !== 13) {
        errors.ruc = "Longitud de ruc incorrecta"
        return
    }
    let provCode = parseInt(number.slice(0, 2))
    if (!((provCode <= 24) || provCode == 30)) {
        errors.ruc = "Código de provincia inválido"
        return
    }
    let end = number.slice(10, 13)
    if (end !== '001') {
        errors.ruc = "Ultimos dígitos inválidos"
        return
    }
    errors.ruc = ""
}
function validatePhone(number: string) {
    if (!(/^\d+$/.test(number))) {
        errors.phone = "Campo solo debe incluir números"
        return
    }
    if (number.length !== 10) {
        errors.phone = "Longitud de teléfono incorrecta"
        return
    }
    errors.phone = ""
    
}
</script>

<template>
    {{ info }}
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
