<template>
    <form ref="formRef" @submit="handleSubmit">
        <predict-input name="content" pred-url="/my/api/endpoint/"></predict-input>

        <button type="submit">Send</button>
    </form>
</template>


<script setup>
import {ref} from 'vue'
import PredictInput from './PredictInput.vue';

const formRef = ref(null)
const handleSubmit = event => {
    event.preventDefault()
    const formData = new FormData(formRef.value)
    const jsonData = Object.fromEntries(formData)
    console.log(jsonData)
}
</script>