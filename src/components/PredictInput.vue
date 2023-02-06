<template>
    <textarea 
        ref="foregroundInput"
        v-model="userDataModel" 
        @keyup="handleKeyUp"
        @keydown.tab="handleTabPress"
        @blur="handleBlur"
        @focus="handleFocus"
        placeholder="Your input.."
        class="input-top"
        rows="10"
        ></textarea>
    <textarea ref="backgroundInput" v-model="predDataModel"  class="input-bottom" readonly rows="10"></textarea>
</template>

<script setup>
import {ref, onMounted} from 'vue'

const foregroundInput = ref(null)
const backgroundInput = ref(null)
const userDataModel = ref("")
const predDataModel = ref("")
const predictedText = ref("this is some of my pred text")

onMounted(()=>{
    styleMatchBackground()
})

const styleMatchBackground = () => {
    backgroundInput.value.style.position = "absolute";
    backgroundInput.value.style.borderColor = "transparent";
    // backgroundInput.value.style.color = "gray";
    backgroundInput.value.style.top = foregroundInput.value.offsetTop + "px";
    backgroundInput.value.style.left = foregroundInput.value.offsetLeft + "px";
    backgroundInput.value.style.width = foregroundInput.value.offsetWidth + "px";
    backgroundInput.value.style.height = foregroundInput.value.offsetHeight + "px";
}
const handleKeyUp = (event) => {
    // predDataModel.value = event.target.value
    predDataModel.value = `${userDataModel.value} ${predictedText.value}`
    styleMatchBackground()
}


const handleTabPress = (event) => {
    event.preventDefault()
    userDataModel.value = `${userDataModel.value} ${predictedText.value}`
    resetPredDataModel()
}

const resetPredDataModel = () => {
    predDataModel.value = userDataModel.value
}

const toggleBackgroundInputTextDisplay = (on) => {
    if (on) {
        // show the text
        backgroundInput.value.style.color = "gray";
    } else {
        // hide the text
        backgroundInput.value.style.color = "transparent";
    }
}

const handleBlur=(event)=>{
    // user clicked away
    toggleBackgroundInputTextDisplay(false)
}

const handleFocus=(event)=>{
    // user clicked in
    toggleBackgroundInputTextDisplay(true)
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