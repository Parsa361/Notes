<script setup>
import { uid } from 'uid';
import { ref, computed } from 'vue';
import BaseModal from './components/BaseModal.vue';

const showModal = ref(false)
const isScaled = ref(false)
const notes = ref([])

function getRandomColor() {
  let color = "hsl(" + Math.random() * 360 + ", 100%, 75%)"
  return color;
}
const toggleModal = () => {
  showModal.value = !showModal.value
  isScaled.value = !isScaled.value
}

const addNote = (inputNote) => {
  notes.value.push({
    // add uid package
    id: uid(),
    text: inputNote,
    date: new Date(),
    backgroundColor: getRandomColor()
  })
  showModal.value = false
}

const truncatedInputNote = computed(() => {
  return (text) => {
    if (text.trim().length > 200) {
      return text.slice(0, 200) + '...'
    } else return text
  }
})
</script>


<template>
  <main>
    <BaseModal :showModal="showModal" @add-note="addNote" @toggle-modal="toggleModal" />

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="toggleModal" :class="{ scaled: isScaled }">+</button>
      </header>

      <div class="cards-container">
        <div v-for="(note, index) in notes" :key="index" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ truncatedInputNote(note.text) }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-US") }}</p>
        </div>

      </div>
    </div>
  </main>
</template>

<style>
body {
  font-family: Arial, Helvetica, sans-serif;
}

main {
  width: 100vw;
  height: 100vh;
}

.container {
  max-width: 1000px;
  padding: 1rem;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-size: 4rem;
  font-weight: bold;
}

header button {
  border: none;
  padding: 1rem;
  width: 3rem;
  height: 3rem;
  cursor: pointer;
  background-color: blueviolet;
  color: white;
  font-size: 1rem;
  text-align: center;
  border-radius: 100%;
}

header button.scaled {
  transform: scale(0.8);
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.main-text {
  overflow: hidden;
  word-wrap: break-word;
}

.date {
  font-size: 1rem;
  font-weight: bold;
}
</style>