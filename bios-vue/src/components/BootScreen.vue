<template>
  <div class="boot-screen">
    <h1>American Megatrends</h1>
    <p>Initializing USB Controllers...</p>
    <p>Detecting Storage Devices...</p>
    <br />
    <p class="hint">Press <b>DEL</b> to enter Setup</p>
  </div>
</template>

<script>
export default {
  mounted() {
    const self = this;

    // 自動進入 OS（丙級開機流程）
    setTimeout(function () {
      self.$emit("boot-finished");
    }, 3000);

    // 監聽 DEL
    window.addEventListener("keydown", this.keyHandler);
  },

  beforeUnmount() {
    window.removeEventListener("keydown", this.keyHandler);
  },

  methods: {
    keyHandler(e) {
      if (e.key === "Delete") {
        this.$emit("enter-bios");
      }
    }
  }
};
</script>

<style>
.boot-screen {
  background: black;
  color: white;
  height: 100vh;
  padding: 30px;
  font-family: monospace;
}
.hint {
  margin-top: 20px;
  color: #ccc;
}
</style>
