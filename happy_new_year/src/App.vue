<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Fireworks } from 'fireworks-js'
import DigitalTimer from './components/DigitalTimer.vue'

const fireworksCanvas = ref(null)
const audioPlayer = ref(null)
let fireworks = null
const isMuted = ref(true)

onMounted(() => {
  fireworks = new Fireworks(fireworksCanvas.value, {
    autoresize: true,
    opacity: 0.5,
    acceleration: 1.05,
    friction: 0.97,
    gravity: 1.5,
    particles: 50,
    traceLength: 3,
    traceSpeed: 10,
    explosion: 5,
    intensity: 30,
    flickering: 50,
    lineStyle: 'round',
    hue: {
      min: 0,
      max: 360
    },
    sound: { enabled: true ,
      files: [
        '/sounds/explosion_1.m4a',
        '/sounds/explosion_3.mp3',
      ],
    },
    delay: {
      min: 30,
      max: 60
    },
    rocketsPoint: {
      min: 50,
      max: 50
    },
    lineWidth: {
      explosion: {
        min: 1,
        max: 3
      },
      trace: {
        min: 1,
        max: 2
      }
    },
    brightness: {
      min: 20,
      max: 100
    },
    decay: {
      min: 0.015,
      max: 0.03
    },
    mouse: {
      click: true,
      move: false,
      max: 5
    }
  })
  fireworks.start()

// Start audio paused and muted
audioPlayer.value.pause()
audioPlayer.value.muted = true

})

onUnmounted(() => {
  if (fireworks) {
    fireworks.stop()
    fireworks = null
  }
  if (audioPlayer.value) {
    audioPlayer.value.pause()
    audioPlayer.value.src = ''
  }
})



const toggleAudio = () => {
  isMuted.value = !isMuted.value
  audioPlayer.value.muted = isMuted.value
  if (!isMuted.value) {
    audioPlayer.value.play()
  } else {
    audioPlayer.value.pause()
  }
}
</script>

<template>
  <div class="min-h-screen bg-black flex flex-col items-center justify-center relative overflow-hidden">
    <div class="z-10 text-center">
      <h1 class="text-6xl md:text-8xl font-bold text-white mb-6 animate-pulse">
        HAPPY NEW YEAR EBRIWAN
      </h1>
      <DigitalTimer class="mb-6" />
      <div class="mb-4">
        <button
          @click="toggleAudio"
          class="px-6 py-3 bg-green-500 text-white text-lg rounded-full hover:bg-green-600 transition duration-300 focus:outline-none focus:ring-2 focus:ring-green-400 focus:ring-opacity-50"
        >
          {{ isMuted ? 'Unmute Audio' : 'Mute Audio' }}
        </button>
      </div>
    </div>
    <canvas ref="fireworksCanvas" class="w-full h-full absolute top-0 left-0 z-0"></canvas>
    <audio ref="audioPlayer" loop>
      <source src="/sounds/happy_new_year.mp3" type="audio/mpeg">
      Your browser does not support the audio element.
    </audio>
    <footer class="absolute bottom-0 w-full bg-black bg-opacity-50 text-white py-4 text-center animate-fade-in-up">
      <p>&copy; {{ new Date().getFullYear() }} Jules. All rights reserved.</p>
    </footer>
  </div>
</template>
