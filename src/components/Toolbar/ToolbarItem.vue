<template>
  <div class="splitbar" v-if="isSplitbar"/>
  <button v-on:click="onClick()" v-bind:id="id" v-bind:title="title" :disabled="isDisabled" v-bind:class="computeClass()" v-else/>
</template>

<script>
export default {
  props: ['id', 'title', 'isSplitbar', 'toRight', 'paletteClass', 'paletteFile', 'paletteEvent', 'disabled', 'active'],
  data: function () {
		return {
			isDisabled: (this.disabled !== undefined),
			isActive: (this.active !== undefined),
			paletteObject: null
		}
	},
	methods: {
		onClick: function () {
			this.$emit('clicked');
		},

		computeClass: function () {
			return (this.toRight ? 'toolbutton pull-right' : 'toolbutton') + (this.isActive ? ' active' : '');
		}
	},
}
</script>

<style>

</style>