<template>
    <textarea 
        ref="foregroundInput"
        v-model="userDataModel" 
        @keyup="handleKeyUp"
        placeholder="Your input.."
        class="input-top"
        ></textarea>
    <textarea ref="backgroundInput" v-model="predDataModel"  class="input-bottom" readonly></textarea>
</template>

<script setup>
import {ref, onMounted} from 'vue'

const foregroundInput = ref(null)
const backgroundInput = ref(null)
const userDataModel = ref("")
const predDataModel = ref("")

onMounted(()=>{
    styleMatchBackground()
})

const styleMatchBackground = () => {
    backgroundInput.value.style.position = "absolute";
    backgroundInput.value.style.borderColor = "transparent";
    // backgroundInput.value.style.color = "transparent";
    backgroundInput.value.style.top = foregroundInput.value.offsetTop + "px";
    backgroundInput.value.style.left = foregroundInput.value.offsetLeft + "px";
    backgroundInput.value.style.width = foregroundInput.value.offsetWidth + "px";
    backgroundInput.value.style.height = foregroundInput.value.offsetHeight + "px";
}
const handleKeyUp = (event) => {
    // predDataModel.value = event.target.value
    predDataModel.value = userDataModel.value
    styleMatchBackground()
}

</script>

<style>

.input-top {
    color: black;
    position: relative;
    background: transparent;
    width: 100%;
    z-index: 2;
}
.input-bottom {
    color: gray;
    position: relative;
    border-color: transparent;
    background: transparent;
    width: 100%;
    z-index: 1;
    resize: none;
}

</style>