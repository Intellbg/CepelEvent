<script setup lang="ts">
import ContactForm from './ContactForm.vue';
import ConsumptionInput from './ConsumptionInput.vue';
import { ref } from 'vue';

const data = ref({ item: { "contact": {}, "2021": {}, "2022": {} } })
const stage = ref(0)
function save() {
    console.log()
}
function load_csv() {

}
function nextInfo(){
    console.log("gaaaaaaaa")
}
</script>

<template>
    <div class="section">
        <div class="form-container">
            <img src="https://cepelee.com/assets/img/logo.jpeg" alt="" class="logo">
            <h1 class="title">Eficiencia energética</h1>
            {{ data.item }}
            <form v-on:submit.prevent>
                <ContactForm v-if='stage == 0' />
                <button class="load_csv" type="button" @click="load_csv()" v-if='stage == 1'> Load csv</button>
                <ConsumptionInput :year="2021" v-if='stage == 1' />
                <ConsumptionInput :year="2022" v-if='stage == 1' />
                <div class="field is-grouped" v-if='stage == 1'>
                    <button class="button is-link green" type="submit">Enviar</button>
                </div>
            </form>
        </div>
    </div>
</template>

<style scoped>
.green {
    background-color: green !important
}

.form-container {
    padding: 2em;
    display: flex;
    flex-direction: column;
    background-color: white;
    width: 80%;
    margin: auto;
}

.logo {
    display: block;
    width: 30%;
}


@media screen and (max-width: 600px) {
    .logo {
        width: 40%;
    }

    .form-container {
        padding: 1em;
        width: 100%;
        margin: auto;
    }

    .section {
        padding-left: 5px;
        padding-right: 5px;
    }
}
</style>
