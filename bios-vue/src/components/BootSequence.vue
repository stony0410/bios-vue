<template>
  <div class="boot-seq">
    <h2>Boot Priority</h2>

    <ul>
      <li
        v-for="(item, index) in list"
        :key="item"
        :class="{ selected: index === selected }"
      >
        Boot Option #{{ index + 1 }} : {{ item }}
      </li>
    </ul>

    <p>[↑/↓] Move Selection</p>
    <p>[+/-] Change Order</p>
    <p>[ESC] Back to BIOS</p>
  </div>
</template>

<script>
export default {
  props: ["bootSequence"],

  data() {
    return {
      list: [...this.bootSequence],
      selected: 0
    };
  },

  mounted() {
    window.addEventListener("keydown", this.keyHandler);
  },

  beforeUnmount() {
    window.removeEventListener("keydown", this.keyHandler);
  },

  methods: {
    keyHandler(e) {
      if (e.key === "ArrowUp" && this.selected > 0) this.selected--;

      if (e.key === "ArrowDown" && this.selected < this.list.length - 1)
        this.selected++;

      if (e.key === "+") {
        if (this.selected > 0) {
          const t = this.list[this.selected];
          this.list[this.selected] = this.list[this.selected - 1];
          this.list[this.selected - 1] = t;
          this.selected--;
          this.$emit("update-sequence", this.list);
        }
      }

      if (e.key === "-") {
        if (this.selected < this.list.length - 1) {
          const t = this.list[this.selected];
          this.list[this.selected] = this.list[this.selected + 1];
          this.list[this.selected + 1] = t;
          this.selected++;
          this.$emit("update-sequence", this.list);
        }
      }

      if (e.key === "Escape") this.$emit("back");
    }
  }
};
</script>

<style>
.boot-seq {
  background: #001a4d;
  height: 100vh;
  color: white;
  padding: 20px;
  font-family: monospace;
}

.selected {
  color: yellow;
}
</style>
