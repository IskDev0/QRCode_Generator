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
      <div class="flex justify-center pt-16">
          <img
              class="
              h-72
              w-72
          p-5
          bg-white
          bs-2"
              v-if="!isLoading"
              :src="isLoading ? 'https://api.qrserver.com/v1/create-qr-code/?data=https://youtu.be/dQw4w9WgXcQ&size=256%D1%85256' : imgSource"
              alt="QRCode">
        <div v-else class="lds-ring"><div></div><div></div><div></div><div></div></div>
        <p v-if="errorMessage && !imgSource.length">{{ errorMessage }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
// 'https://api.qrserver.com/v1/create-qr-code/?data=https://youtu.be/dQw4w9WgXcQ&size=256%D1%85256'

import {ref} from "vue";

const text = ref("")
let imgSource = ref("")
const errorMessage = ref("")
let isLoading = ref(false)

const generateQRCode = async () => {
  try {
    isLoading.value = true
    const response = await fetch(`https://api.qrserver.com/v1/create-qr-code/?data=${text.value}&size=256х256`)
    imgSource.value = response.url
    text.value = ""
  } catch (err) {
    errorMessage.value = "Input is empty"
  } finally {
    isLoading.value = false
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