<script setup>
import { reactive } from 'vue'
import { ref } from 'vue'

const seeds = reactive([])
const seedCopy = ref([])
const newSeed = ref('')
const combinationLimit = ref(0)
const ideas = ref([])

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

const generateCombinations = function () {
  console.log('combining')
  
  const res = []

  function backtrack(start, comb) {
    console.log('backtracking!')
    if (comb.length === combinationLimit.value) {
      console.log('base case')
      res.push(Array.from(comb));
      return
    }

    for (let i = start; i < seeds.length; i++) {
      comb.push(i);
      backtrack(i +1, comb);
      comb.pop();
    }
  }

  backtrack(0, []);

  console.log(res)
  return res;
}

const createIdeas = function (e) {
  e.preventDefault();

  if (seeds.length < 2) {
    return
  }
  ideas.value = []
  seedCopy.value = Array.from(seeds)
  ideas.value = generateCombinations()
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

  <div class="py-2 mb-4">
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

    <div>
      <p 
        v-for="(idea, ind) in ideas" 
        :key="ind+idea"
      >
        <span 
          v-for="(el, ind) in idea"
          :key="el+ind+idea"
        >
          {{ seedCopy[el] }},
        </span>
      </p>
    </div>
  </div>
</div>
</template>

<style>
</style>
