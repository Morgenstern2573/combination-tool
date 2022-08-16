<script setup>
import { reactive } from 'vue'
import { ref } from 'vue'

const seeds = reactive([])
const newSeed = ref('')
const combinationLimit = ref(0)

const addSeed = function (e) {
  e.preventDefault()
  
  if (seeds.includes(newSeed.value)) {
    return
  }

  seeds.push(newSeed.value)
  newSeed.value = ""
}

const removeSeed = function (ind) {
  seeds.splice(ind, 1)
}
</script>

<template>
<div>
  <h1>Brainstorm</h1>

  <form 
    @submit="addSeed"
    class="mb-4"
  >
    <input 
      type="text"
      class="border px-5"
      v-model="newSeed"
      @keydown.enter="addSeed"
      required
      pattern="^([a-zA-z]| |-|[1-9])+$"
    >

    <button 
      class="bg-gray-800 text-white ml-2 inline-block rounded px-2 py-1 shadow"
    >
      Add seed
    </button>
  </form>

  <div class="py-2">
    <span 
      v-for="(el, ind) in seeds" :key="ind"
      class="rounded-full border px-5 py-1"
    >
      <span>{{ el }}</span>
      
      <span 
        @click="removeSeed(ind)"
        class="ml-2 inline-block cursor-pointer"
      >
        X
      </span>
    </span>
  </div>

  <form 
    class="mb-4"
    @submit="createIdeas"
  >
    <input 
      type="number"
      class="border px-5"
      v-model="combinationLimit"
      min="2"
      @keydown.enter="createIdeas"
      required
    >
    <button
      class="bg-gray-800 text-white ml-2 inline-block rounded px-2 py-1 shadow"
    >
      Brainstorm
    </button>
  </form>

  <div>
    <h2>Ideas</h2>
  </div>
</div>
</template>

<style>
</style>
