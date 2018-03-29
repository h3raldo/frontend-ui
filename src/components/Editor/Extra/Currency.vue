<template>
	<div class="editor_extra_currency">
		<div class="form-group">
			<label class="form-label" for="currency">Currency</label>
			<input class="form-input" type="text" id="currency" placeholder="USD">
		</div>
	</div>
</template>
<script>
export default {
  name: 'Currency',
  props: {
		current: Object
  },
	mounted() {
		this.$emit(
			'set-extra',
			{
				name:'currency',
				value: 'USD'
			}
		)
	},
}
</script>
