<template>
  <div id="main-toolbar" class="toolbar">
    <toolbar-item id="activity-button" :title="l10n.stringChessActivity"></toolbar-item>
    <toolbar-item isSplitbar="true"></toolbar-item>
    
    <toolbar-item ref="restartBtn" id="settings-button" :class="{ 'active': settingsOpen }" @clicked="$emit('settingsClicked')" :title="l10n.stringSettings"></toolbar-item>
    <toolbar-item ref="addBtn" id="add-button" v-if="settingsOpen" :title="l10n.stringAddCat" data-toggle="modal" data-target="#addCategory"></toolbar-item>
    <toolbar-item ref="reorderBtn" id="reorder-button" :class="{ 'active': catSortable }" @clicked="$emit('reorderClicked')" v-if="settingsOpen" :title="l10n.stringReorderCat"></toolbar-item>
    <toolbar-item ref="downloadBtn" id="download-button" @clicked="$emit('downloadClicked')" v-if="!settingsOpen" :title="l10n.stringDownload"></toolbar-item>
    <toolbar-item isSplitbar="true"></toolbar-item>

    <toolbar-item ref="networkBtn" id="network-button" :title="l10n.stringNetwork"></toolbar-item>

    <toolbar-item @clicked="" id="stop-button" title="Stop" toRight="true"></toolbar-item>
    <toolbar-item ref="fullscreen" @clicked="" id="fullscreen-button" :title="l10n.stringFullscreen" toRight="true"></toolbar-item>
    <toolbar-item @clicked="" id="help-ui-button" :title="l10n.stringHelp" toRight="true"></toolbar-item>
  </div>
</template>

<script>
import ToolbarItem from './ToolbarItem';

export default {
  components: {
    'toolbar-item': ToolbarItem
  },
  props: ['settingsOpen', 'catSortable'],
	data: function () {
		return {
			l10n: {
				stringChessActivity: '',
				stringTemplate: '',
				stringSettings: 'Settings Mode',
				stringHelp: 'Help',
				stringDownload: 'Download',
				stringAddCat: 'Add Category',
        stringReorderCat: 'Reorder Categories',
				stringFullscreen: 'Fullscreen',
				stringNetwork: 'Network (To share the skills with others using Presence)',
				stringDifficulty: '',
				stringRules: '',
			}
		}
	},
}
</script>

<style>
.toolbar {
  height: 55px;
  padding: 0 2%;
  color: white;
  background-color: #282828;
  -webkit-user-select: none;
  -moz-user-select: none;
}
.toolbar .toolbutton {
  position: relative;
  width: 47px;
  height: 47px;
  margin: 4px;
  color: white;
  color: transparent;
  background-color: transparent;
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
  border: 0;
  border-radius: 5.5px;
}
button {
  -webkit-appearance: button;
  -webkit-writing-mode: horizontal-tb !important;
  text-rendering: auto;
  letter-spacing: normal;
  word-spacing: normal;
  text-transform: none;
  text-indent: 0px;
  text-shadow: none;
  display: inline-block;
  text-align: center;
  align-items: flex-start;
  cursor: default;
  background-color: rgb(239, 239, 239);
  box-sizing: border-box;
  margin: 0em;
  font: 400 13.3333px Arial;
  padding: 1px 6px;
  border-width: 2px;
  border-style: outset;
  border-color: rgb(118, 118, 118);
  border-image: initial;
  border-radius: 2px;
  padding: 2px 4px;
  line-height: 22px;
  color: white;
  background-color: #808080;
  border: 2px solid transparent;
  border-radius: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
}
.toolbar .toolbutton:hover, .toolbar .active {
  background-color: black;
}
.pull-right {
  float: right;
}

.splitbar {
	background-image: url('../../assets/icons/split.svg');
	background-position: center;
	background-repeat: no;
	width: 3px;
	margin-left: 5px;
	margin-right: 4px;
	margin-bottom: 4px;
	height: 45px;
	display: inline-block;
	vertical-align: bottom;
}

#fullscreen-button {
	background-image: url('../../assets/icons/view-fullscreen.svg');
	outline: none;
}

#unfullscreen-button {
	background-image: url('../../assets/icons/view-return.svg');
	border: 0;
	border-radius: 0px;
	margin: 1px;
	width: 45px;
	height: 45px;
	top: 0;
	right: 5%;
	position: absolute;
	visibility: hidden;
	z-index: 5;
	outline: none;
}

#main-toolbar #activity-button {
	background-image: url('../../assets/icons/owner-icon.svg');
	outline: none;
}

#main-toolbar #reorder-button {
	background-image: url('../../assets/icons/reorder.svg');
	outline: none;
}

#main-toolbar #add-button {
	background-image: url('../../assets/icons/plus.svg');
	outline: none;
}

#main-toolbar #restart-button {
	background-image: url('../../assets/icons/restart.svg');
	outline: none;
}

#main-toolbar #menu-button {
	background-image: url('../../assets/icons/menu.svg');
	outline: none;
}

#main-toolbar #play-button {
	background-image: url('../../assets/icons/play.svg');
	outline: none;
}

#main-toolbar #hint-button {
	background-image: url('../../assets/icons/idea.svg');
	outline: none;
}

#main-toolbar #download-button {
	background-image: url('../../assets/icons/download.svg');
	outline: none;
}

#main-toolbar #undo-button {
	background-image: url('../../assets/icons/edit-undo.svg');
	outline: none;
}

#main-toolbar #undo-button {
	background-image: url('../../assets/icons/edit-undo.svg');
	outline: none;
}

#main-toolbar #difficulty-button {
	background-image: url('../../assets/icons/robot-on.svg');
	outline: none;
}

#main-toolbar #settings-button {
	background-image: url('../../assets/icons/settings.svg');
	outline: none;
}

#main-toolbar #help-ui-button {
	background-image: url('../../assets/icons/help.svg');
	outline: none;
}

#main-toolbar #help-rules-button {
	background-image: url('../../assets/icons/help-rules.svg');
	outline: none;
}

#main-toolbar #stop-button {
	outline: none;
}

#main-toolbar #network-button {
  background-image: url('../../assets/icons/zoom-home.svg');
}
#private-button {
  background-image: url('../../assets/icons/zoom-home.svg');
  width: 47px;
  height: 47px;
  margin: 4px 2px;
  color: white;
  color: transparent;
  background-color: transparent;
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
  border: 0;
  border-radius: 5.5px;
}
#shared-button {
  background-image: url('../../assets/icons/zoom-neighborhood.svg');
  width: 47px;
  height: 47px;
  margin: 4px 2px;
  color: white;
  color: transparent;
  background-color: transparent;
  background-position: center;
  background-repeat: no-repeat;
  background-size: contain;
  border: 0;
  border-radius: 5.5px;
}

</style>