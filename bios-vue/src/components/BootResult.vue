<template>
  <div class="boot-result">
    <h2>
      {{
        bootSequence[0] === "USB-HDD"
          ? "Booting from USB..."
          : "Booting from HDD..."
      }}
    </h2>

    <p>Press Enter to reboot</p>
  </div>
</template>

<script>
export default {
  props: ["bootSequence"],

  mounted() {
    window.addEventListener("keydown", this.keyHandler);
  },

  beforeUnmount() {
    window.removeEventListener("keydown", this.keyHandler);
  },

  methods: {
    keyHandler(e) {
      if (e.key === "Enter") this.$emit("reboot");
    }
  }
};
</script>

<style>
.boot-result {
  background: black;
  color: white;
  height: 100vh;
  font-family: monospace;
  padding: 40px;
}
</style>
