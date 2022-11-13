<template>
  <div
      class="
  bg-gradient-to-r from-sky-400 to-blue-500
  min-h-screen
  pt-16
p-8"
  >
    <div class="container text-xl mx-auto">
      <form @submit.prevent="generateQRCode" class="flex flex-col gap-4">
        <input
            v-model="text"
            type="text"
            class="
        form-control
        block
        w-full
        px-3
        py-1.5
        text-base
        font-normal
        text-gray-700
        bg-white bg-clip-padding
        border border-solid border-white
        rounded
        transition
        ease-in-out
        m-0
        focus:text-gray-700 focus:bg-white focus:outline-none
      "
            placeholder="Enter text..."
        />
        <button
            class="
          rounded-md
          border-white
          border-2
          text-white
          uppercase">Generate
        </button>
      </form>
      <div class="flex flex-col items-center justify-center pt-16">
        <img
            class="
              h-72
              w-72
          p-5
          bg-white
          bs-2"
            v-if="isLoading && imgSource.length > 0"
            :src="show ? '../assets/error.webp' : imgSource "
            alt="QRCode">
        <div v-if="show" class="lds-ring">
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
        <div v-if="error && !imgSource.length" class="flex bg-blue-100 rounded-lg p-4 mb-4 text-sm text-blue-700" role="alert">
          <svg class="w-5 h-5 inline mr-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"></path></svg>
          <div>
            <span class="font-medium">Input is empty</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// 'https://api.qrserver.com/v1/create-qr-code/?data=https://youtu.be/dQw4w9WgXcQ&size=256%D1%85256'

import {ref} from "vue";

let text = ref("")
let imgSource = ref("")
let error = ref(false)
let isLoading = ref(false)
let show = ref(false)

const generateQRCode = async () => {
  try {
    isLoading.value = false
    show.value = true
    const response = await fetch(`https://api.qrserver.com/v1/create-qr-code/?data=${text.value}&size=256х256`)
    imgSource.value = response.url
    text.value = ""
  } catch (err) {
    error.value = true
  } finally {
    isLoading.value = true
    show.value = false
  }
}
</script>

<style>
.lds-ring {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}

.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 64px;
  height: 64px;
  margin: 8px;
  border: 8px solid #fff;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: #fff transparent transparent transparent;
}

.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}

.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}

.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}

@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

</style>