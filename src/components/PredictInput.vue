<template>
    <textarea 
        :name="props.name"
        ref="foregroundInput"
        v-model="userDataModel" 
        @keyup="handleKeyUp"
        @keydown="handleKeyDown"
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
import {ref, onMounted, defineEmits, defineProps} from 'vue'

const emit = defineEmits(['didChange'])
const props = defineProps(['name', 'endpoint', 'skip-checks'])
const foregroundInput = ref(null)
const backgroundInput = ref(null)
const endpointReady = ref(false)
const userDataModel = ref("")
const predDataModel = ref("")
const predReqTimeout = ref(null)
const predictionSamples = [
    "this is cool",
    "ready",
    "let's do this",
    "nice work",
    "neat",
    "I like what you did there",
    "random is fun"
] 

const predictedText = ref("")

onMounted(async ()=>{
    styleMatchBackground()
    await checkEndpoint()
    
})

const checkEndpoint = async () => {
    if (props.endpoint) {
        const response = await fetch(props.endpoint)
        const contentType = response.headers.get('content-type')
        if (response.ok) {
            if (contentType === "json") {
                console.log("ready")
                endpointReady.value = true
            } else {
                console.log('not ready')
            }
        }
    }
}

const styleMatchBackground = () => {
    backgroundInput.value.style.position = "absolute";
    backgroundInput.value.style.borderColor = "transparent";
    // backgroundInput.value.style.color = "gray";
    backgroundInput.value.style.top = foregroundInput.value.offsetTop + "px";
    backgroundInput.value.style.left = foregroundInput.value.offsetLeft + "px";
    backgroundInput.value.style.width = foregroundInput.value.offsetWidth + "px";
    backgroundInput.value.style.height = foregroundInput.value.offsetHeight + "px";
}

const handleKeyDown = event => {
    predDataModel.value = `${userDataModel.value}`
}

const handleKeyUp = (event) => {
    // predDataModel.value = event.target.value
    
    requestPrediction()
    styleMatchBackground()
    emit('didChange', {
        "value": userDataModel.value,
        "prediction": predictedText.value
    })
}

const requestPrediction = ()=> {
    clearTimeout(predReqTimeout.value)
    predReqTimeout.value = setTimeout(()=>{
        // this is our HTTP request
        if (endpointReady.value) {
            const randomIdx = Math.floor(Math.random() * predictionSamples.length)
            const randomPredValue = predictionSamples[randomIdx]
            predictedText.value = randomPredValue
            if (userDataModel.value === "") {
                predDataModel.value  = ""
            } else {
                predDataModel.value = `${userDataModel.value} ${randomPredValue}`
            }
        }
    }, 500)
    
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