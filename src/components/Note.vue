<template>
  <div
    class="accordion__item"
    :style="{
      'background-color': note.theme,
      opacity: getOpacity(note.opacity),
    }"
  >
    <div class="accordion__item__header">
      <div class="accordion__item__btn-groups">
        <span class="accordion__item__btn" @click.prevent="toggleNote(note)">
          <i :class="note.isLocked ? 'fas fa-lock' : 'fas fa-lock-open'" />
        </span>
        <span class="accordion__item__btn" @click.prevent="deleteNote(index)">
          <i class="fas fa-times" />
        </span>
      </div>
      <span>{{ note.title }}</span>
    </div>
    <div
      class="accordion__item__contents"
      :class="{ 'is-locked': note.isLocked }"
    >
      <p>{{ note.text }}</p>
      <div>
        <label>투명도</label>
        <input v-model="note.opacity" type="range" min="0" max="100" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "note",
  props: {
    note: Object,
    toggleNote: Function,
    deleteNote: Function,
  },
  methods: {
    getOpacity(value) {
      if (typeof value === "number") {
        return value / 100;
      } else if (typeof value === "string" && !isNaN(Number(value))) {
        return Number(value) / 100;
      }
      return 1;
    },
  },
};
</script>

<style>
.accordion__item {
  position: relative;
  margin: 16px 0;
  padding: 16px;
  box-sizing: border-box;
}

.accordion__item__header {
}

.accordion__item__contents {
  transition: max-height 0.25s ease-in;
  max-height: 500px;
  overflow: hidden;
}

.accordion__item__contents.is-locked {
  transition: max-height 0.15s ease-out;
  max-height: 0;
}

.accordion__item__btn-groups {
  position: absolute;
  top: 0;
  right: 0;
}

.accordion__item__btn {
  margin: 8px;
}
</style>