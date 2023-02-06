<template>
    <textarea 
        v-model="userDataModel" 
        @keyup="handleKeyUp"
        placeholder="Your input.."
        ></textarea>
    <textarea v-model="predDataModel" readonly></textarea>
</template>

<script setup>
import {ref} from 'vue'

const userDataModel = ref("")
const predDataModel = ref("")

const handleKeyUp = (event) => {
    // predDataModel.value = event.target.value
    predDataModel.value = userDataModel.value
}

</script>