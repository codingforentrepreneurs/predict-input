<template>
    <textarea 
        v-model="userDataModel" 
        @keyup="handleKeyUp"
        placeholder="Your input.."
        class="input-top"
        ></textarea>
    <textarea v-model="predDataModel"  class="input-bottom" readonly></textarea>
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

<style>

.input-top {
    color: black;
    position: relative;
    background: transparent;
    width: 100%;
    z-index: 2;
    resize: none;
}
.input-bottom {
    color: gray;
    position: relative;
    border-color: transparent;
    background: transparent;
    width: 100%;
    z-index: 2;
}

</style>