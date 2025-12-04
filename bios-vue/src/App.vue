<!-- App.vue：超初學者版本，語法簡單易懂 -->

<template>
  <!-- Boot 開機畫面 -->
  <BootScreen
    v-if="currentScreen === 'boot'"
    @enter-bios="enterBios"
    @boot-finished="goToOS"
  />

  <!-- BIOS 主畫面 -->
  <BiosMain
    v-else-if="currentScreen === 'bios'"
    :boot-sequence="bootSequence"
    @open-boot-seq="openBootSequenceMenu"
    @save-exit="saveAndReboot"
    @exit-nosave="cancelAndReboot"
  />

  <!-- Boot Sequence 子選單 -->
  <BootSequence
    v-else-if="currentScreen === 'boot-seq'"
    :boot-sequence="bootSequence"
    @update-sequence="updateBootSequence"
    @back="backToBiosMenu"
  />

  <!-- OS 啟動結果 -->
  <BootResult
    v-else-if="currentScreen === 'os'"
    :boot-sequence="savedBootSequence"
    @reboot="reboot"
  />
</template>

<script>
import BootScreen from "./components/BootScreen.vue"
import BiosMain from "./components/BiosMain.vue"
import BootSequence from "./components/BootSequence.vue"
import BootResult from "./components/BootResult.vue"

export default {
  name: "App",

  components: {
    BootScreen,
    BiosMain,
    BootSequence,
    BootResult
  },

  data() {
    return {
      // 現在顯示哪一個畫面
      // boot → bios → boot-seq → os
      currentScreen: "boot",

      // 使用者正在調整中的開機順序
      bootSequence: ["HDD-0", "CDROM", "USB-HDD"],

      // 實際 BIOS 設定（按 F10 才會更新）
      savedBootSequence: ["HDD-0", "CDROM", "USB-HDD"]
    }
  },

  methods: {
    // Boot 畫面按 DEL
    enterBios() {
      this.currentScreen = "bios"
    },

    // 開機動畫跑完
    goToOS() {
      this.currentScreen = "os"
    },

    // BIOS 主畫面 → 進入 Boot Sequence 子選單
    openBootSequenceMenu() {
      this.currentScreen = "boot-seq"
    },

    // 子選單那邊按 + 或 -，更新開機順序
    updateBootSequence(newList) {
      this.bootSequence = [...newList]
    },

    // ESC 回到 BIOS 主畫面
    backToBiosMenu() {
      this.currentScreen = "bios"
    },

    // F10（存檔）→ 儲存 → 回到開機畫面
    saveAndReboot() {
      this.savedBootSequence = [...this.bootSequence]
      this.currentScreen = "boot"
    },

    // ESC（不存）→ 把資料還原 → 回到開機畫面
    cancelAndReboot() {
      this.bootSequence = [...this.savedBootSequence]
      this.currentScreen = "boot"
    },

    // OS 畫面按重開機
    reboot() {
      this.currentScreen = "boot"
    }
  }
}
</script>

<style>
body {
  margin: 0;
}
</style>
