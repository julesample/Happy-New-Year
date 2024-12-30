<template>
    <div class="text-4xl md:text-6xl font-bold text-white mb-6">
      {{ formattedTime }}
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted } from 'vue'
  
  const formattedTime = ref('00:00:00')
  
  const updateTimer = () => {
    const now = new Date()
    const midnight = new Date(now.getFullYear(), now.getMonth(), now.getDate() + 1)
    const diff = midnight - now
  
    const hours = Math.floor(diff / 3600000).toString().padStart(2, '0')
    const minutes = Math.floor((diff % 3600000) / 60000).toString().padStart(2, '0')
    const seconds = Math.floor((diff % 60000) / 1000).toString().padStart(2, '0')
  
    formattedTime.value = `${hours}:${minutes}:${seconds}`
  }
  
  let timer
  
  onMounted(() => {
    updateTimer()
    timer = setInterval(updateTimer, 1000)
  })
  
  onUnmounted(() => {
    clearInterval(timer)
  })
  </script>
  
  