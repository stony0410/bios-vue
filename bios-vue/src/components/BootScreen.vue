<><template>
  <div class="boot-screen">
    <h2>Booting...</h2>
    <p>Press DEL to enter BIOS</p>
  </div>
</template>

<script>
export default {
  name: "BootScreen",

  mounted() {
    var self = this

    // 模擬開機畫面 3 秒後自動進入 OS
    setTimeout(function () {
      self.$emit("boot-finished")
    }, 3000) 

    // 監聽鍵盤事件：按 DEL (keyCode 46)
    window.addEventListener("keydown", this.keyHandler)
  },

  beforeUnmount() {
    window.removeEventListener("keydown", this.keyHandler)
  },

  methods: {
    keyHandler(event) {
      if (event.key === "Delete") {
        this.$emit("enter-bios")
      }
    }
  }
}
</script>

<style>
.boot-screen {
  background: black;
  color: white;
  height: 100vh;
  padding: 20px;
}
</style>
