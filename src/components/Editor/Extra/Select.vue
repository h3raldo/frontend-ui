<template>
	<div class="editor_extra_select">
		<div
			v-for="(option,index) in current.extra.options"
			v-bind:key="option.position"
			v-bind:option="option">

			<div class="columns">
				<div class="column col-5">
					<div class="form-group">
						<label class="form-label">Name</label>
						<input class="form-input" type="text" placeholder="Name" v-model="option.name">
					</div>
				</div>
				<div class="column col-5">
					<div class="form-group">
						<label class="form-label">Value</label>
						<input class="form-input" type="text" placeholder="Value" v-model="option.value">
					</div>
				</div>
				<div class="column col-2" v-if="option.position > 1">
					<div class="form-group">
						<label class="form-label">&nbsp;</label>
						<button class="btn btn-error" v-on:click="removeOption(index)">Remove</button>
					</div>
				</div>
			</div>

		</div>

		<div class="mt-2">
			<button class="btn btn-secondary" v-on:click="addOption()"><i class="icon icon-plus"></i> Add Option</button>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Select',
	props: {
		current: Object
	},
	mounted() {
		this.$emit(
			'set-extra',
			{
				name:'options',
				value: [
					Object.assign({}, this.new_option)
				]
			}
		);
	},
	data: function() {
		return {
			new_option: {
				position: 1, name: '', value: ''
			}
		}
	},
	methods: {
		/**
		 * Adds an option to the array of select options
		 */
		addOption: function() {
			let options = this.current.extra.options;
			let next_position = options[options.length-1].position + 1;
			let new_option = Object.assign( {}, this.new_option )
			new_option.position = next_position;

			this.current.extra.options.push( new_option );
		},
		/**
		 * Remove an option from the array of select options
		 *
		 * @param {int} index Index of the option to be removed
		 */
		removeOption: function( index ) {
			this.current.extra.options.splice(index, 1);
		}
	}
}
</script>
