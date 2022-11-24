<script setup lang="ts">
import ContactForm from './ContactForm.vue';
import ConsumptionForm from './ConsuptionForm.vue'
import { ref } from 'vue';

const data = ref({ "contact": {}, "2021": {}, "2022": {} })
const stage = ref(getStage())
function getStage() {
    if (localStorage.getItem("stage")) {
        return parseInt(localStorage.getItem("stage") || "")
    }
    return 0
}
function getContact(info: object) {
    data.value.contact = info
    stage.value++
}
async function getData(d1: any, d2: any) {
    const resp = await fetch(
        "https://sot1yngvm2.execute-api.us-east-1.amazonaws.com/default/CepelEvent",
        {
            method: "POST",
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ "data": { "2021": d1.value, "2022": d2.value }, ...data.value.contact })
        }
    )
    localStorage.setItem("stage", "2")
    stage.value++
}

</script>

<template>
    <div class="section">
        <div class="form-container">
            <img src="https://cepelee.com/assets/img/logo.jpeg" alt="" class="logo">
            <h1 class="title">Eficiencia energética</h1>
            <form v-on:submit.prevent>
                <ContactForm @nextInfo="getContact" v-if='stage==0' />
                <ConsumptionForm @getConsumptionData="getData" v-if='stage == 1' />
            </form>
            <div class="" v-if='stage == 2'>
                <h2 class="title is-5">Gracias por su información</h2>
                <div class="is-flex">
                    <h3 class="title is-6">Visite los recursos recursos del evento:
                        <a class="tag is-link"
                            href="https://epnecuador-my.sharepoint.com/:f:/g/personal/roberto_burbano_epn_edu_ec/EktsvS9TzRJLtv6YSLbClbQBSR0l5PzNnYDjvhnZoA0x8A?e=mg2yir">Drive</a>
                    </h3>
                </div>
                <br>
                <div class="is-flex">
                    <h3 class="title is-6 ">No olvide de visitar nuestra página web:
                        <a class="tag is-link green has-text-centered" href="https://cepelee.com/">CEPEL</a>
                    </h3>
                </div>
            </div>
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
