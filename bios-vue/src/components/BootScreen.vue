<script>
export default {
  methods: {
    keyHandler(e) {
      // 進 BIOS
      if (e.key === "Delete") {
        this.$emit("enter-bios")
      }

      // 開機跑完 → 進 OS
      if (e.key === "Enter") {
        this.$emit("boot-finished")
      }
    }
  },

  mounted() {
    window.addEventListener("keydown", this.keyHandler)
  },

  beforeUnmount() {
    window.removeEventListener("keydown", this.keyHandler)
  }
}
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
