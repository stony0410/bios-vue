<template>
  <div class="bios">

    <!-- 上方 Tabs -->
    <div class="tabs">
      <span :class="{active: currentTab === 'Main'}">Main</span>
      <span :class="{active: currentTab === 'Advanced'}">Advanced</span>
      <span :class="{active: currentTab === 'Power'}">Power</span>
      <span :class="{active: currentTab === 'Boot'}">Boot</span>
      <span :class="{active: currentTab === 'Tools'}">Tools</span>
      <span :class="{active: currentTab === 'Exit'}">Exit</span>
    </div>

    <!-- 主內容區：依 Tab 顯示不同頁面 -->
    <div class="main">

      <!-- Main 頁面：Date / Time + SATA -->
      <MainPage v-if="currentTab === 'Main'" @open-sata="openSata" />

      <!-- Boot 頁面（你原本的選單） -->
      <div v-if="currentTab === 'Boot'" class="menu">
        <div
          v-for="(item, index) in bootMenu"
          :key="index"
          :class="['item', { selected: index === selected }]"
        >
          ▶ {{ item }}
        </div>
      </div>

      <!-- 右側 Help 區 -->
      <div class="help">
        <p>Use [↑] or [↓] to select an item</p>
        <p>Use [Enter] to open</p>
        <p>Use [←] or [→] to switch tabs</p>
        <br />
        <p>F10 : Save & Exit</p>
        <p>ESC : Exit</p>
      </div>
    </div>

  </div>
</template>

<script>
import MainPage from "./MainPage.vue";

export default {
  components: { MainPage },

  props: ["bootSequence"],

  data() {
    return {
      currentTab: "Boot",   // 啟動時停在 Boot
      selected: 0,

      bootMenu: [
        "Hard Disk Boot Priority",
        "USB Boot Device",
        "First Boot Device",
        "Second Boot Device",
        "Third Boot Device"
      ]
    };
  },

  mounted() {
    window.addEventListener("keydown", this.handleKey);
  },
  beforeUnmount() {
    window.removeEventListener("keydown", this.handleKey);
  },

  methods: {
    handleKey(e) {
      // 左右切換 Tab
      const tabs = ["Main", "Advanced", "Power", "Boot", "Tools", "Exit"];
      let i = tabs.indexOf(this.currentTab);

      if (e.key === "ArrowLeft") {
        i = (i - 1 + tabs.length) % tabs.length;
        this.currentTab = tabs[i];
      }

      if (e.key === "ArrowRight") {
        i = (i + 1) % tabs.length;
        this.currentTab = tabs[i];
      }

      // Boot 頁面：上下移動選單
      if (this.currentTab === "Boot") {
        if (e.key === "ArrowUp" && this.selected > 0) this.selected--;
        if (e.key === "ArrowDown" && this.selected < this.bootMenu.length - 1) this.selected++;

        if (e.key === "Enter") {
          if (this.selected === 0) this.$emit("open-boot-seq"); // Boot Priority
          if (this.selected === 1) this.$emit("open-usb-device"); // USB Boot
        }
      }

      // F10 = save
      if (e.key === "F10") {
        this.$emit("save-exit");
      }

      // ESC = cancel
      if (e.key === "Escape") {
        this.$emit("exit-nosave");
      }
    },

    openSata() {
      this.$emit("open-sata");
    }
  }
};
</script>

<style>
/* BIOS 外觀 */
.bios {
  background: #d6d9df;
  height: 100vh;
  color: #00008b;
  font-family: monospace;
}

/* Tabs */
.tabs {
  background: #204a87;
  color: white;
  padding: 6px 20px;
  display: flex;
  gap: 25px;
  font-size: 18px;
}

.tabs span.active {
  background: white;
  color: #204a87;
  padding: 3px 8px;
  border-radius: 4px;
  font-weight: bold;
}

/* 主區塊 */
.main {
  display: flex;
  padding: 20px;
  height: calc(100% - 40px);
}

/* 左側選單 */
.menu {
  width: 60%;
}

.item {
  padding: 6px;
  user-select: none;
}

.item.selected {
  background: #00008b;
  color: white;
  font-weight: bold;
}

/* 右側 Help */
.help {
  width: 40%;
  border-left: 2px solid #00008b;
  padding-left: 20px;
}
</style>
