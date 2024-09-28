<template>
  <div class="container mt-4">
    <h1 class="text-center">Notes</h1>
    <form @submit.prevent="addNote" class="mb-3">
      <div class="form-group">
        <input
          type="text"
          v-model="newNote"
          placeholder="Enter a note"
          class="form-control"
          required
        />
      </div>
      <button type="submit" class="btn btn-primary mt-2">New Note</button>
    </form>

    <ul class="list-group">
      <li
        v-for="(note, index) in notes"
        :key="index"
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <span>{{ note }}</span>
        <div>
          <button class="btn btn-warning btn-sm" @click="editNote(index)">
            Edit
          </button>
          {{'  '}}
          <button
            class="btn btn-danger btn-sm ml-2"
            @click="deleteNote(index)"
          >
            Delete
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newNote: '',
      notes: [],
    };
  },
  mounted() {
    this.loadNotes();
  },
  methods: {
    addNote() {
      if (this.newNote.trim() !== '') {
        this.notes.push(this.newNote);
        this.newNote = '';
        this.saveNotes();
      }
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
      this.saveNotes();
    },
    editNote(index) {
      this.newNote = this.notes[index];
      this.deleteNote(index);
    },
    saveNotes() {
      localStorage.setItem('notes', JSON.stringify(this.notes));
    },
    loadNotes() {
      const savedNotes = localStorage.getItem('notes');
      if (savedNotes) {
        this.notes = JSON.parse(savedNotes);
      }
    },
  },
};
</script>

<style>
.container {
  max-width: 600px;
}
</style>
