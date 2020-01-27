<template>
  <div>
    <div class="noteBloc">
      <textarea type="text" v-model="message" />
      <button @click="addNote">Submit</button>
    </div>
    <div id="notes">
      <div v-bind:class="getClass(note.index)" v-bind:key="note.index" v-for="note in notes">
        <div class="remove">
          <span>{{ note.creationDate }}</span>
          <img v-bind:title="removeTitle" @click="removeNote(note.index)" src="../assets/bin.svg" />
        </div>
        <p v-bind:title="focusTitle" @click="handleFocus(note.index)">{{ note.description }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  props: {
    msg: String
  },
  data() {
    return {
      notes: [],
      message: null,
      removeTitle: 'Delete this note',
      focusTitle: 'Select this note',
      selectedNote: -1
    };
  },
  methods: {
    addNote() {
      if (this.message) {
        this.notes.push({
          index: this.notes.length,
          description: this.message,
          creationDate: new Date().toLocaleString('es-ES')
        });
        localStorage.notes = JSON.stringify(this.notes);
        this.message = null;
      }
    },
    removeNote(index) {
      if (index > -1) {
        const noteIndex = this.notes.findIndex(note => note.index === index);
        if (noteIndex > -1) {
          this.notes.splice(noteIndex, 1);
          localStorage.notes = JSON.stringify(this.notes);
        }
      }
    },
    handleFocus(index) {
      if (index > -1) {
        const noteIndex = this.notes.findIndex(note => note.index === index);
        if (noteIndex > -1) {
          this.selectedNote = index;
        }
      }
    },
    getClass(index) {
      return `note${this.selectedNote === index ? ' selected' : ''}`;
    }
  },
  mounted() {
    if (localStorage.notes) {
      this.notes = JSON.parse(localStorage.notes);
    }
  }
};
</script>

<style scoped lang="scss">
.noteBloc {
  width: 16.2rem;
  display: grid;
  padding: 5px;
  margin: 7px;
  textarea {
    margin-bottom: 0.5rem;
    min-height: 50px;
    max-height: 250px;
    resize: vertical;
  }
  button {
    background: none;
    padding: 5px;
    border: 1px solid;
    border-radius: 10px;
  }
}
#notes {
  width: 100%;
  display: flex;
  flex-flow: column wrap;
  max-height: 800px;
  justify-content: flex-start;
  align-content: flex-start;
  .note {
    width: 15rem;
    border: 1px solid;
    border-radius: 15px;
    padding: 4px 10px 10px 10px;
    margin: 10px;
    height: fit-content;
    border-color: darkgray;
    p {
      cursor: pointer;
    }
    .remove {
      width: 100%;
      display: inline-flex;
      align-items: flex-end;
      font-size: 12px;
      justify-content: space-between;
      img {
        cursor: pointer;
      }
    }
  }
  .selected {
    border-color: grey;
    border-width: 2px;
    margin: 9px;
  }
}
</style>
