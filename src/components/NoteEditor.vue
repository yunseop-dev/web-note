<template>
  <div class="note-grid">
    <div class="note-editor">
      <input
        class="title-input"
        type="text"
        v-model="title"
        placeholder="Title"
      />
      <textarea
        rows="10"
        v-model="text"
        placeholder="Take a note..."
      ></textarea>
      <div class="note-editor-bottom">
        <div>
          <compact-picker v-model="colors" />
        </div>
        <button @click="createNew">
          <i class="fas fa-check-circle"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { Compact } from "vue-color";

export default {
  data: function () {
    return {
      title: "",
      text: "",
      colors: {},
    };
  },
  components: {
    "compact-picker": Compact,
  },
  methods: {
    createNew() {
      this.$emit("noteAdded", this.title, this.text, this.colors.hex);
      this.title = "";
      this.text = "";
    },
    deleteNote(index) {
      this.$emit("noteDeleted", index);
    },
  },
};
</script>
