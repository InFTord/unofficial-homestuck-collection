<template>
    <div id="titleBar">
      <div id="titleBarText" v-text="activeTabTitle" />
      <div id="titleBarButtons" tabindex="-1" v-if="showButtons">
        <div class="systemButton" id="minButton" @click="minimize()" >‒</div>
        <div class="systemButton" id="maxButton" @click="maximize()" >☐</div>
        <div class="systemButton" id="closeButton" @click="close()" >✕</div>
    </div>
    </div>
</template>

<script>
const { ipcRenderer } = require('electron')
export default {
  name: 'titleBar',
  components: {

  },
  data(){
    return {
    }
  },
  computed: {
    showButtons() {
      //Only show buttons if the navigator doesn't report a mac system
      return navigator.appVersion.indexOf('Macintosh') == -1
    },
    activeTabTitle() {
      let title = this.$archive ? this.$localData.tabData.tabs[this.activeTabKey].title : 'The Unofficial Homestuck Collection'
      document.title = title
      return title
    },
    activeTabKey() {
      return this.$localData.tabData.activeTabKey
    }
  },
  methods: {
    minimize() {
      ipcRenderer.invoke('win-minimize')
    },
    maximize() {
      ipcRenderer.invoke('win-maximize')
    },
    close() {
      ipcRenderer.invoke('win-close')
    }
  },
  mounted() {
  }
}
</script>

<style lang="scss" scoped>
  #titleBar{
    background: var(--header-bg);
    color: var(--font-header);

    min-height: 18px;
    margin-top: 4px;
    -webkit-app-region: drag;

    * {
      user-select: none;
    }
  }
  #titleBarText{
    text-align: center;
    white-space: nowrap;
    overflow: hidden;
    width: 85%;
    margin: 0 auto;
    padding-bottom: 2px;
  }
  #titleBarButtons {
    -webkit-app-region: no-drag;
    position: fixed;
    top: 0;
    right: 0;
    display: flex;
    .systemButton {
      display: inline-block;
      pointer-events: auto;
      width: 25px;
      height: 22px;

      line-height: 24px;
      padding: 0 2px;
    }
    #closeButton{
      &:hover {
        color: white;
				background: rgb(255, 73, 73);
			}
    }
  }
</style>
