<template>
	<div class="form-group">
		<label class="form-label" for="custom_validation">Custom Validation with RegEx</label>
		<input class="form-input" type="text" id="custom_validation" placeholder="Name" v-model="current.custom_validation">
		<p v-if="isValidRegex() === false">Error: Invalid regex pattern</p>
		<div v-else-if="current.custom_validation.length > 0">
			<div class="form-group">
				<label class="form-label" for="custom_validation_test">Test your RegEx</label>
				<input class="form-input" type="text" id="custom_validation_test" placeholder="Test string" v-model="test_input">
				<template v-if="test_input.length > 0">
				<p v-if="regexMatchesTest() === true">String validates!</p>
				<p v-else>String does not validate</p>
				</template>
			</div>
		</div>
	</div>
</template>
<script>
export default {
	name: 'CustomValidation',
	props: {
		current: Object,
	},
	data: function(){
		return{
			// locally stored test input string - doesn't need to be stored anywhere so we're using local component data
			test_input: ''
		}
	},
	methods: {
		/**
		 * Checks if the user's pattern validates as RegEx
		 *
		 * @returns {boolean}
		 */
		isValidRegex: function()
		{
			let regex = this.current.custom_validation;

			try {
				new RegExp(regex);
			} catch(e) {
				return false;
			}

			return true;
		},

		/**
		 * Tests the test input against the user's RegEx pattern
		 *
		 * @returns {boolean}
		 */
		regexMatchesTest: function()
		{
			let regex = new RegExp( this.current.custom_validation );
			return regex.test( this.test_input );
		}
	}
}
</script>