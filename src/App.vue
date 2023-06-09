<script setup lang="ts">
  import { ref, onMounted } from 'vue'
  import type { Ref } from 'vue'

  import GameRules from './components/GameRules.vue'

  type Attempt = {
    number: number[],
    bulls: number,
    cows: number
  }

  let secretNumber: Ref<number[]> = ref([])

  let newGuess: Ref<number[]> = ref([])

  function addNumber (number: number) {
    newGuess.value.push(number)
  }

  function removeNumber () {
    newGuess.value.pop()
  }

  let attempts: Ref<Attempt[]> = ref([])

  let isWinner: Ref<boolean> = ref(false)

  onMounted(() => {
    startNewGame()

    window.addEventListener("keydown", (e) => {
      if (e.key === 'Enter') {
        e.preventDefault()

        if (isWinner.value) {
          startNewGame()
        } else if (newGuess.value.length === 4) {
          addGuess()
        }

        return
      }

      if(e.key === 'Backspace') {
        removeNumber()

        return
      }

      const key = +e.key
      if (key >= 0 && key <= 9) { 
        addNumber(key)
      }
    })
  })

  function startNewGame () {
    const newNumber: Set<number> = new Set()
    while (newNumber.size < 4) {
      const digit = Math.floor(0 + Math.random() * 9)
      newNumber.add(digit)
    }
    
    secretNumber.value = Array.from(newNumber)

    newGuess.value = []
    attempts.value = []
    isWinner.value = false
  }

  function addGuess () {
    let cows = 0
    let bulls = 0

    newGuess.value.forEach((n, index) => {
      if (secretNumber.value.includes(n)) {
        
        if (secretNumber.value[index] === n) {
          bulls +=1
        } else {
          cows += 1
        }
      }
    });

    attempts.value.push({
      number: [...newGuess.value],
      cows,
      bulls
    })

    newGuess.value = []

    // check winner
    if (bulls === 4) {
      isWinner.value = true
    }
  }

</script>

<template>
  <div class="container mx-auto py-8 px-4 text-center">
    <h1 class="text-3xl mb-4">Try to guess</h1>
    <p class="mb-6">
      <button 
        class="bg-green-700 hover:bg-green-800 border-b-4 border-green-800 hover:border-green-900 text-xl tracking-wider text-white font-bold py-4 px-6 rounded"
        type="button" 
        @click="startNewGame"
      >New game
      </button>
    </p>
    <h2
      v-show="isWinner"
      class="text-green-600 font-bold text-2xl my-4"
    >
      You win!!!
    </h2>
    <div class="relative overflow-x-auto mb-6 max-w-5xl mx-auto">
      <table class="border-collapse	border-spacing-0 table-auto w-full">
        <thead>
          <tr>
            <th
              class="border p-2"
            >#</th>
            <th
              class="border p-2"
            >Your guess</th>
            <th
              class="border p-2"
            >Cows</th>
            <th
              class="border p-2"
            >Bulls</th>
          </tr>
        </thead>
        <tbody v-if="attempts.length">
          <tr v-for="(attempt, i) in attempts">
            <td 
              class="border p-2"
            >
              {{ i + 1 }}
            </td>
            <td 
              class="border p-2"
            >
              {{ attempt.number.join('') }}
            </td>
            <td 
              class="border p-2"
            >
              {{ attempt.cows }}
            </td>
            <td 
              class="border p-2"
            >
              {{ attempt.bulls }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <form>
      <h2 class="font-bold mb-4 text-2xl">
        Your number 
        <span v-for="number in newGuess">{{ number }}</span>
      </h2>
      <div class="max-md:flex max-md:justify-center max-md:align-center">
        <p class="mb-4 md:space-x-0.5 max-md:order-1 max-md:space-y-0.5">
          <button 
            type="submit" 
            class="bg-green-700 hover:bg-green-800 disabled:bg-gray-500 text-white font-bold border border-green-700 disabled:border-gray-400 py-2 px-4 rounded max-md:w-full" 
            :disabled="isWinner || newGuess.length !== 4"
            @click.prevent="addGuess"
          >GO</button>
          <button
            class="bg-white hover:bg-gray-100 disabled:bg-gray-500 text-gray-800 disabled:text-white font-semibold py-2 px-4 border border-gray-400 rounded max-md:w-full"
            type="button"
            :disabled="isWinner || !newGuess.length"
            @click="removeNumber"
          >
            Remove
          </button>
        </p>
        <p class="space-x-0.5 space-y-0.5">
          <button 
            v-for="i in 10"
            :key="i"
            class="bg-sky-600 hover:bg-sky-800 disabled:bg-gray-500 text-white font-bold py-2 px-4 rounded max-md:w-1/6"
            :disabled="isWinner || newGuess.length >= 4 || newGuess.includes(i - 1)"
            type="button"
            @click="addNumber(i-1)"
          >
              {{ i - 1 }}
          </button>
        </p>
      </div>
    </form>

    <game-rules />
    
  </div>
</template>
