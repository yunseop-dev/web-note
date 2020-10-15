<template>
  <div id="app">
    <app-header @openEditor="editorOpen = !editorOpen"></app-header>
    <app-note-editor
      v-if="editorOpen"
      @noteAdded="newNote"
      @noteDeleted="deleteNote"
    ></app-note-editor>
    <div class="accordion">
      <note
        v-for="(note, index) in notes"
        :key="`note-${index}`"
        :note="note"
        :toggleNote="toggleNote"
        :deleteNote="deleteNote"
      />
    </div>
  </div>
</template>

<script>
import NoteEditor from "./components/NoteEditor.vue";
import Header from "./components/Header.vue";
import Note from "./components/Note.vue";

export default {
  name: "App",
  data: function () {
    return {
      editorOpen: false,
      notes: [],
    };
  },
  computed: {},
  methods: {
    newNote(title, text, theme) {
      this.notes.push({
        title: title,
        text: text,
        theme: theme,
        opacity: 100,
        isLocked: false,
        password: "",
      });
    },
    deleteNote(index) {
      this.notes.splice(index, 1);
    },
    toggleNote(note) {
      if (note.isLocked) {
        this.unlockNote(note);
      } else {
        this.lockNote(note);
      }
    },
    lockNote(note) {
      const password = prompt("비밀번호 입력하세요.");
      if (!password) {
        alert("내용이 없습니다!");
        return;
      }
      const password2 = prompt("비밀번호 다시 한번 입력하세요.");
      if (password !== password2) {
        alert("비밀번호가 맞지 않습니다!");
        return;
      }
      note.password = password;
      note.isLocked = true;
    },
    unlockNote(note) {
      const password = prompt("비밀번호 입력하세요.");
      if (!password) {
        alert("내용이 없습니다!");
        return;
      } else if (note.password === password) {
        note.isLocked = false;
        note.password = "";
      } else {
        alert("비밀번호가 맞지 않습니다!");
      }
    },
  },
  mounted() {
    if (localStorage.getItem("notes")) {
      this.notes = JSON.parse(localStorage.getItem("notes"));
    }
  },
  watch: {
    notes: {
      handler() {
        var newNotes = this.notes;
        localStorage.setItem("notes", JSON.stringify(newNotes));
      },
      deep: true,
    },
  },
  components: {
    appNoteEditor: NoteEditor,
    appHeader: Header,
    Note
  },
};
</script>

<style lang="scss">
@import "/styles/global.scss";
.accordion {
  max-width: 600px;
  margin: 0 auto;
}
</style>
