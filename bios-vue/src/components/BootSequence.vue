<template>
  <div class="boot-seq">
    <h2>Boot Sequence Settings</h2>

    <ul>
      <li
        v-for="(item, index) in list"
        :key="item"
        :style="{ color: index === selected ? 'yellow' : 'white' }"
      >
        {{ item }}
      </li>
    </ul>

    <p>↑ ↓ 選擇裝置</p>
    <p>+ 往上移</p>
    <p>- 往下移</p>
    <p>[ESC] 回到 BIOS 主選單</p>
  </div>
</template>

<script>
export default {
  name: "BootSequence",

  props: ["bootSequence"],

  data() {
    return {
      list: [...this.bootSequence],
      selected: 0
    }
  },

  mounted() {
    window.addEventListener("keydown", this.keyHandler)
  },

  beforeUnmount() {
    window.removeEventListener("keydown", this.keyHandler)
  },

  methods: {
    keyHandler(e) {
      // 選擇上下移動
      if (e.key === "ArrowUp" && this.selected > 0) {
        this.selected--
      } else if (e.key === "ArrowDown" && this.selected < this.list.length - 1) {
        this.selected++
      }

      // + 往上
      else if (e.key === "+") {
        if (this.selected > 0) {
          var temp = this.list[this.selected]
          this.list[this.selected] = this.list[this.selected - 1]
          this.list[this.selected - 1] = temp
          this.selected--
          this.$emit("update-sequence", this.list)
        }
      }

      // - 往下
      else if (e.key === "-") {
        if (this.selected < this.list.length - 1) {
          var t = this.list[this.selected]
          this.list[this.selected] = this.list[this.selected + 1]
          this.list[this.selected + 1] = t
          this.selected++
          this.$emit("update-sequence", this.list)
        }
      }

      // ESC 回 BIOS
      else if (e.key === "Escape") {
        this.$emit("back")
      }
    }
  }
}
</script>

<style>
.boot-seq {
  background: #000066;
  color: white;
  height: 100vh;
  padding: 20px;
}
</style>
