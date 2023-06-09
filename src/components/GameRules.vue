<template>
  <button
    class="absolute top-2 right-2 p-1 border border-gray-600 rounded"
    title="Show rules"
    @click="isRulesVisible = !isRulesVisible"
  >
    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
      <path stroke-linecap="round" stroke-linejoin="round" d="M11.25 11.25l.041-.02a.75.75 0 011.063.852l-.708 2.836a.75.75 0 001.063.853l.041-.021M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9-3.75h.008v.008H12V8.25z" />
    </svg>
  </button>

  <div 
    v-show="isRulesVisible" 
    class="fixed top-0 left-0 min-h-full min-w-full flex justify-center items-center"
  >
    <div 
      class="fixed top-0 left-0 min-h-full min-w-full bg-gray-400/50"
      @click="isRulesVisible = false"
    ></div>
    <div class="overflow-y-auto max-h-screen w-full p-4">
      <div class="bg-white max-w-lg w-full m-auto px-4 pb-4 pt-5 sm:p-6 sm:pb-4 space-y-4 shadow-lg shadow-gray-500 rounded z-5 relative text-left">
        <h4 class="text-xl text-center font-bold">How to play</h4>
        <p>Bulls and Cows is a 2 player game. One player thinks of a number, while the other player tries to guess it.</p>
        <ul class="list-disc list-inside space-y-2">
          <li>The number to be guessed must be a 4 digit number, using only digits from 1 - 9, each digit atmost once. e.g. 1234 is valid, 0123 is not valid, 9877 is not valid, 9876 is valid.</li>
          <li>For every guess that the player makes, he gets 2 values - the number of bulls and the number of cows. 1 bull means the guess contains and the target number have 1 digit in common, and in the correct position. 1 cow means the guess and the target have 1 digit in common, but not in correct position. e.g. Let the target be 1234. Guessing 4321 will give 0 bulls and 4 cows. 3241 will give 1 bull and 3 cows. 4 bulls means you have won the game!</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'

const isRulesVisible: Ref<boolean> = ref(false)
</script>