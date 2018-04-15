<template>

	<div id="app">

		<Navbar></Navbar>

		<div class="form-container">

			<div class="form-header">
				<h1>Commercial Property - Add Field</h1>
			</div>

			<div class="form-builder mb-2" v-bind:class="{loading: view.isLoading}">
				<div class="container">
					<div class="columns">
						<div class="column col-3 col-md-12 relative">
							<Selector
								:user="user"
								:types="app.input_types"
								:current="view.current"
								@new-input="newInput($event)">
							</Selector>
						</div>
						<div class="column col-9 col-md-12 editor_column relative">
							<Editor
								:view="view"
								:user="user"
								:app="app"
								@set-extra="setExtra($event)">
							</Editor>
						</div>
					</div>
				</div>
			</div>

			<div class="form-actions">
				<button class="btn btn-primary btn-lg" @click="saveInput" v-bind:class="{ 'disabled': (!canSaveInput() && !view.isLoading), 'loading': view.isLoading }">
					<template v-if="canSaveInput() || view.isLoading">Save Changes</template>
					<template v-else>Missing Required Fields</template>
				</button>

				<button class="btn btn-error btn-lg float-right" @click="saveInput">Delete Input</button>
				<button class="btn btn-default btn-lg float-right mr-2" @click="saveInput">Cancel Changes</button>
			</div>
		</div>
	</div>

</template>

<script>
import Vue from 'vue'
import Navbar from "./components/Navbar.vue"
import Selector from "./components/Selector.vue"
import Editor from './components/Editor.vue'

export default {
	name: 'app',
	mounted() 
	{
		// retrieve the user data. i'm using local storage as our DB
		if( localStorage.getItem('user') !== null ){
			this.user = JSON.parse( localStorage.getItem('user') );
		}

		// set up a new blank input when first mounted
		this.newInput('text');
	},
	methods: {
		/**
		 * Checks required fields are filled in so that the input can be saved
		 *
		 * @returns {boolean}
		 */
		canSaveInput: function()
		{
			if( !this.view.current.display_label ){
				return false;
			}
			if( !this.view.current.reference_name ){
				return false;
			}

			return true;
		},
		/**
		 * Adds a new blank input to the current view. Overwrites
		 * any current unsaved data in the view
		 *
		 * @param {string} type Type of field to add
		 */
		newInput: function( type )
		{
			// clone new input and reset extra and tags
			var input = Object.assign( {}, this.app.new_input );
			input.type = type;
			input.extra = {};
			input.tags = [];

			this.view.current = input;
		},
		/**
		 * Sets extra data on an input view. This is data that 
		 * isn't standardized and may be different based on input
		 * type, so it's manually added when needed
		 *
		 * @param {object} extra Object in format { name: 'prop_name_here', value: []|{}\'' }
		 */
		setExtra: function( extra )
		{
			Vue.set( this.view.current.extra, extra.name, extra.value );
		},

		/**
		 * Adds the input from the current view to the user's stored inputs
		 */
		saveInput: function()
		{
			let self = this;

			// start loading state for user feedback
			this.view.isLoading = true;

			this.user.inputs.push( Object.assign({}, this.view.current) );

			// save user data to local storage
			localStorage.setItem('user', JSON.stringify(this.user) );

			// reset the current view
			this.newInput( 'text' );

			// fake a loading time for this test
			setTimeout( function(){ 
				self.view.isLoading = false; 
			}, 1000);
		}

	},
	data: function() {
		return {
			// data used for the app variables that should not change
			app: {
				input_types: [
					{
						name: 'Text',
						code: 'text',
						description: 'String of text',
						default_display: 'Free-form text input',
						exclude: []
					},
					{
						name: 'Number',
						code: 'number',
						description: 'Number input box',
						default_display: 'Free-form number type input',
						exclude: []
					},
					{
						name: 'Date',
						code: 'date',
						description: 'Standard ISO format date',
						default_display: 'Datepicker, with configurable format',
						exclude: []
					},
					{
						name: 'Currency',
						code: 'currency',
						description: 'Number input with currency format',
						default_display: 'Test',
						exclude: []
					},
					{
						name: 'Select',
						code: 'select',
						description: 'Select for a list of options',
						default_display: 'Standard dropdown with multiple options',
						exclude: [
							'custom_validation', 'default_value'
						]
					}
				],
				tags: [
					{
						name: "Vinmaster",
						id: "vinmaster",
						types: [ 'text' ],
						requires: [
							{ name: 'VIN' }
						],
						returns: [
							{ name: 'Car Make' }
						]
					},
					{
						name: "Housemaster",
						id: "housemaster",
						types: [ 'text' ],
						requires: [
							{ name: 'Address' }
						],
						returns: [
							{ name: 'House Type' }
						]
					}
				],
				new_input: {
					type: 'text',
					reference_name: '',
					display_label: '',
					default_value: '',
					custom_validation: '',
					tags: [],
					field_group: {},
					extra: {}
				},
				new_group: {
					id: 1,
					name: "",
					edit: true
				}

			},

			// data for the current interface, this is where we'd work on the input to be saved for the user
			view: {
				// current input being worked on
				current: {},
				isLoading: false
			},

			// saved user data, data that would be saved to the DB. In this case it's all of the saved inputs and groups
			user: {
				// array of saved input groups
				groups: [],
				// array of saved inputs
				inputs: []
			}
		}
	},
	components: {
		Navbar, Selector, Editor
	}
}
</script>