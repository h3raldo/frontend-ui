<template>
	<div class="editor_default">
		<div class="form-group">
			<label class="form-label" for="display_label">Display Label</label>
			<input class="form-input" type="text" id="display_label" placeholder="Name" v-model="current.display_label">
		</div>

		<div class="form-group">
			<label class="form-label" for="reference_name">Reference Name</label>
			<input class="form-input" type="text" id="reference_name" placeholder="Name" v-model="current.reference_name">
		</div>

		<div class="form-group">
			<label class="form-label" for="default_value">Default Value</label>
			<input class="form-input" type="text" id="default_value" placeholder="Name" v-model="current.default_value">
		</div>

		<div class="form-group">
			<label class="form-label" for="custom_validation">Custom Validation</label>
			<input class="form-input" type="text" id="custom_validation" placeholder="Name" v-model="current.custom_validation">
		</div>

		<Tags :current="current" :types="types"></Tags>
	</div>
</template>
<script>
import Tags from './Tags.vue'
export default {
  name: 'Default',
  props: {
		current: Object,
		types: Array
  },
	components: {
		Tags
	}
}
</script>
