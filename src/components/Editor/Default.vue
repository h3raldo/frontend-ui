<template>
	<div class="editor_default">

		<div class="columns">
			<div class="column col-6 col-xs-12">
				<div class="form-group">
					<label class="form-label" for="display_label">Display Label</label>
					<input class="form-input" type="text" id="display_label" placeholder="Name" v-model="current.display_label" @blur="generateReferenceName()">
					<p class="form-input-hint">For display purposes, spaces allowed</p>
				</div>
			</div>
			<div class="column col-6 col-xs-12">
				<div class="form-group" :class="{ 'has-error': !isReferenceUnique() }">
					<label class="form-label" for="reference_name">Reference Name</label>
					<input class="form-input" type="text" id="reference_name" placeholder="Name" v-model="current.reference_name">
					
					<template v-if="!isReferenceUnique()">
						<p class="form-input-hint">Another input has this reference, please choose a unique reference.</p>
					</template>
					<template v-else>
						<p class="form-input-hint">Used to reference in calculations, no spaces allowed</p>
					</template>
				</div>
			</div>
		</div>

		<div class="columns">
			<div class="column col-6 col-xs-12">
				<div class="form-group" v-if="shouldShowInput('default_value')">
					<label class="form-label" for="default_value">Default Value</label>
					<input class="form-input" type="text" id="default_value" placeholder="Name" v-model="current.default_value">
				</div>
			</div>
			<div class="column col-6 col-xs-12">
				<template v-if="shouldShowInput('custom_validation')">
					<CustomValidation :current="current"></CustomValidation>
				</template>
			</div>
		</div>

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
		tags: Array,
		inputs: Array
	},
	methods: {
		/**
		 * Generates the reference name based on the display label
		 */
		generateReferenceName: function()
		{
			let label = this.current.display_label;
			// make sure something was written
			if( label.length < 1 ) return;
			let reference_name = 'ref_'+ label
			// replace whitespace with _, remove non alphanumeric characters, lowercase it
			reference_name = reference_name.replace(/ /g,'_').replace(/\W/g, '').toLowerCase();
			this.current.reference_name = reference_name;
		},
		/**
		 * Make sure the reference name is unique
		 *
		 * @returns {boolean}
		 */
		isReferenceUnique: function()
		{
			// if blank, no need to check
			if( !this.current.reference_name || this.current.reference_name.length < 1 ){
				return true;
			}

			let self = this;
			let matched = this.inputs.filter(function(input){
				return input.reference_name === self.current.reference_name
			});

			return ( matched.length < 1 );
		},
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
