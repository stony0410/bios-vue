<template>
  <div class="bios-main">
    <!-- 左側選單 -->
    <div class="menu">
      <div class="title">BIOS Utility</div>
      <ul>
        <li class="active">Boot</li>
        <li>Security</li>
        <li>Advanced</li>
        <li>Save & Exit</li>
      </ul>
    </div>

    <!-- 右側內容 -->
    <div class="content">
      <h2>Boot Priority</h2>

      <ul class="boot-list">
        <li v-for="item in bootSequence" :key="item">
          {{ item }}
        </li>
      </ul>

      <div class="help">
        <p>[Enter] Edit Boot Sequence</p>
        <p>[F10] Save Changes & Exit</p>
        <p>[ESC] Exit Without Saving</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
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
        this.$emit("open-boot-seq")
      } else if (e.key === "F10") {
        this.$emit("save-exit")
      } else if (e.key === "Escape") {
        this.$emit("exit-nosave")
      }
    }
  }
};
</script>

<style>
.bios-main {
  display: flex;
  width: 100%;
  height: 100vh;
  background: #0a3d62; /* Win10 BIOS 藍 */
  color: white;
  font-family: Arial;
}

/* 左側選單 */
.menu {
  width: 250px;
  background: rgba(255, 255, 255, 0.05);
  padding: 20px;
  border-right: 2px solid #1e90ff;
}

.menu .title {
  font-size: 22px;
  font-weight: bold;
  margin-bottom: 20px;
}

.menu ul {
  list-style: none;
  padding: 0;
}

.menu li {
  padding: 10px;
  margin: 5px 0;
  cursor: pointer;
}

.menu li.active {
  background: #1e90ff;
  border-radius: 5px;
}

/* 右側內容 */
.content {
  flex-grow: 1;
  padding: 30px;
}

.boot-list {
  list-style: none;
  padding: 0;
  font-size: 20px;
}

.boot-list li {
  margin-bottom: 10px;
  padding: 8px 15px;
  border: 1px solid #1e90ff;
  width: 300px;
  border-radius: 5px;
}

.help {
  margin-top: 30px;
  font-size: 14px;
  opacity: 0.9;
}
</style>
