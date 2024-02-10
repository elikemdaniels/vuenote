<script setup>
import { ref } from 'vue';
const showModal = ref(false);
const newNote = ref('');
const errorMessage = ref('');
const notes = ref([]);

// Reset state function
const resetStates = () => {
  showModal.value = false;
  newNote.value = '';
  errorMessage.value = '';
};

// Generate random HSL light background colours
const getRandomColour = () => {
  const hue = Math.floor(Math.random() * 12000);
  const saturation = 65;
  const lightness = 90;
  return `hsl(${hue}, ${saturation}%, ${lightness}%)`;
};

// Formatted Date
const formattedDate = () => {
  const date = new Date();
  const options = { year: 'numeric', month: 'short', day: 'numeric' };
  return new Intl.DateTimeFormat('en-US', options).format(date);
};

// Add note function
const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = 'Note must be at least 10 characters');
  } else {
    notes.value.push({
      id: Math.floor(Math.random() * 1000000),
      text: newNote.value,
      date: formattedDate(),
      backgroundColour: getRandomColour(),
      deleteButton: false,
    });
    resetStates();
  }
};

// Show delete button per note
const showDeleteButton = (note) => {
  note.deleteButton = true;
};

// Show delete button per note
const hideDeleteButton = (note) => {
  note.deleteButton = false;
};

// Delete note from the array
const deleteNote = (id) => {
  notes.value = notes.value.filter((note) => note.id !== id);
};
</script>

<template>
  <main class="container mx-auto max-w-7xl px-12">
    <header class="flex items-center justify-between py-8">
      <h1 class="text-6xl font-bold">Vuenotes</h1>
      <button @click="showModal = true" class="add-note-button">
        <i class="pi pi-plus"></i>
      </button>
    </header>
    <div v-if="showModal" class="overlay">
      <div class="modal flex flex-col p-[4%]">
        <textarea
          v-model="newNote"
          class="h-40 w-full rounded-2xl border border-slate-400"
          name="note"
          id="note"
        ></textarea>
        <p class="text-red-600">{{ errorMessage }}</p>
        <button
          @click="addNote"
          class="mt-5 w-full rounded-2xl bg-slate-950 p-4 text-slate-50"
        >
          Add note
        </button>
        <button
          @click="showModal = false"
          class="mt-5 w-full rounded-2xl border border-slate-300 bg-slate-50 p-4 text-slate-950"
        >
          Close
        </button>
      </div>
    </div>
    <div v-if="notes.length >= 1" class="cards-container">
      <div
        @mouseover="showDeleteButton(note)"
        @mouseleave="hideDeleteButton(note)"
        class="card"
        v-for="note in notes"
        :key="note.id"
        :style="{ background: note.backgroundColour }"
      >
        <p class="note">{{ note.text }}</p>
        <div
          class="card-footer mt-auto flex w-full items-center justify-between"
        >
          <p class="date">{{ note.date }}</p>
          <button v-if="note.deleteButton" @click="deleteNote(note.id)">
            <i class="pi pi-trash"></i>
          </button>
        </div>
      </div>
    </div>
    <div v-if="notes.length === 0" class="no-cards-empty-state">
      <h2 class="mt-20 text-2xl">No added note</h2>
      <p>Add a new note</p>
    </div>
  </main>
</template>
