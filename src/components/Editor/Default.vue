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

		<div class="form-group" v-if="shouldShowInput('default_value')">
			<label class="form-label" for="default_value">Default Value</label>
			<input class="form-input" type="text" id="default_value" placeholder="Name" v-model="current.default_value">
		</div>

		<template v-if="shouldShowInput('custom_validation')">
			<CustomValidation :current="current"></CustomValidation>
		</template>

		<Tags :current="current" :tags="tags"></Tags>
	</div>
</template>
<script>
import Tags from './Tags.vue'
import CustomValidation from './Default/CustomValidation.vue'

export default {
	name: 'Default',
	props: {
		current: Object,
		types: Array,
		tags: Array
	},
	methods: {
		/**
		 * Checks the field type's excluded fields to determine
		 * if the input should be visible for current field type
		 *
		 * @param {string} input Name of the input to be checked
		 * @returns {boolean}
		 */
		shouldShowInput: function( input )
		{
			let excluded;
			let self = this;
			let type = self.types.filter(function(type){
				return type.code === self.current.type;
			});

			if ( type.length !== 1 ){
				return;
			}

			type = type[0];

			excluded = type.exclude.filter(function(exclude){
				return exclude === input;
			});

			return( excluded.length < 1 )
		}
	},
	components: {
		CustomValidation, Tags
	}
}
</script>
