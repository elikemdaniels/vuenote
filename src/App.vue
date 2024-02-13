<script setup>
import { ref } from 'vue';
// const showModal = ref(false);
// const newNote = ref('');
// const errorMessage = ref('');
const notes = ref([]);

// Reset state function
// const resetStates = () => {
//   showModal.value = false;
//   newNote.value = '';
//   errorMessage.value = '';
// };

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

// Add empty note function
const addEmptyNote = () => {
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: '',
    date: formattedDate(),
    backgroundColour: getRandomColour(),
    isEditing: true,
    deleteButton: false,
  });
};

// Save note function
const saveNote = (note) => {
  if (note.text.length < 10) {
    alert('Note must be at least 10 characters'); // Simple alert for validation message
  } else {
    note.isEditing = false;
  }
};

// Edit note function
const editNote = (note) => {
  note.isEditing = true;
};

// Delete note from the array
const deleteNote = (id) => {
  notes.value = notes.value.filter((note) => note.id !== id);
};

// Show delete button per note
const showDeleteButton = (note) => {
  note.deleteButton = true;
};

// Show delete button per note
const hideDeleteButton = (note) => {
  note.deleteButton = false;
};
</script>

<template>
  <div class="container mx-auto max-w-7xl px-12">
    <header class="flex items-center justify-between py-8">
      <h1 class="text-6xl font-bold">Vuenotes</h1>
      <button @click="addEmptyNote" class="add-note-button">
        <i class="pi pi-plus"></i>
      </button>
    </header>
    <div v-if="notes.length >= 1" class="cards-container">
      <div
        @mouseover="showDeleteButton(note)"
        @mouseleave="hideDeleteButton(note)"
        class="card"
        v-for="note in notes"
        :key="note.id"
        :style="{ background: note.backgroundColour }"
      >
        <div class="flex min-h-full w-full flex-col" v-if="note.isEditing">
          <textarea
            v-model="note.text"
            class="note-textarea h-[80%] bg-transparent"
          ></textarea>

          <div class="card-footer">
            <p @click="saveNote(note)" class="text-sm font-semibold">
              Save note
            </p>
            <button
              v-if="note.deleteButton"
              @click.stop="deleteNote(note.id)"
              class="delete-note-button"
            >
              <i class="pi pi-trash"></i>
            </button>
          </div>
        </div>
        <div
          class="flex min-h-full w-full flex-col"
          v-else
          @click="editNote(note)"
        >
          <p class="note-text h-[80%] overflow-y-auto">{{ note.text }}</p>
          <div class="card-footer">
            <p class="text-sm font-semibold">{{ note.date }}</p>
            <button
              v-if="note.deleteButton"
              @click.stop="deleteNote(note.id)"
              class="delete-note-button"
            >
              <i class="pi pi-trash"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="notes.length === 0" class="no-cards-empty-state">
      <h2 class="mt-20 text-2xl">No added note</h2>
      <p>Add a new note</p>
    </div>
  </div>
</template>
