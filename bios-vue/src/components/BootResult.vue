<template>
  <div class="os-screen">
    <h2>OS Loading...</h2>

    <p>Boot Order Used:</p>
    <ul>
      <li v-for="item in bootSequence" :key="item">{{ item }}</li>
    </ul>

    <p>[Enter] Reboot</p>
  </div>
</template>

<script>
export default {
  name: "BootResult",

  props: ["bootSequence"],

  mounted() {
    window.addEventListener("keydown", this.keyHandler)
  },

  beforeUnmount() {
    window.removeEventListener("keydown", this.keyHandler)
  },

  methods: {
    keyHandler(e) {
      if (e.key === "Enter") {
        this.$emit("reboot")
      }
    }
  }
}
</script>

<style>
.os-screen {
  background: #003300;
  color: white;
  height: 100vh;
  padding: 20px;
}
</style>
