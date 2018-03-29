<template>

	<div id="app">
		<h1>Commercial Property - Add Field</h1>

		<div class="columns">
			<div class="column col-4">
				<Selector
					:types="app.input_types"
					@new-input="newInput($event)">
				</Selector>
			</div>
			<div class="column col-8">

				<Editor
					:view="view"
					:user="user"
					:app="app"
					@set-extra="setExtra($event)">
				</Editor>

				<button @click="saveInput">Save Input</button>
			</div>
		</div>
	</div>

</template>

<script>
import Vue from 'vue'
import Selector from "./components/Selector.vue"
import Editor from './components/Editor.vue'

export default {
	name: 'app',
	components: {
		Selector, Editor
	},
	methods: {

		newInput: function( type )
		{
			// clone new input and reset extra and tags
			var input = Object.assign( {}, this.app.new_input );
			input.type = type;
			input.extra = {};
			input.tags = [];

			this.view.current = input;
		},

		setExtra: function( extra ){
			Vue.set( this.view.current.extra, extra.name, extra.value );
		},

		// saves the view input to the user
		saveInput: function(){

			this.user.inputs.push( Object.assign({}, this.view.current) );

			// reset the current view
			this.newInput( 'text' );
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
						description: 'This is for text',
						tags: [
							{
								name: "ANSA"
							}
						]
					},
					{
						name: 'Number',
						code: 'number',
						description: 'This is for numbers',
						tags: []
					},
					{
						name: 'Date',
						code: 'date',
						description: 'This is for dates!',
						tags: []
					},
					{
						name: 'Currency',
						code: 'currency',
						description: 'This is for currenct',
						tags: []
					},
					{
						name: 'Select',
						code: 'select',
						description: 'This is for multi-items',
						tags: []
					}
				],
				new_input: {
					type: 'text',
					reference_name: 'Reference name thing',
					display_label: 'display label',
					default_value: 'default value',
					custom_validation: '.@#AS/',
					tags: [],
					field_group: '',
					extra: {}
				},

			},

			// data for the current interface, this is where we'd work on the input to be saved for the user
			view: {
				// current input being worked on
				current: {
					type: 'text',
					reference_name: 'Reference name thing',
					display_label: 'display label',
					default_value: 'default value',
					custom_validation: '.@#AS/',
					tags: [],
					field_group: '',
					extra: {}
				}
			},

			// saved user data, data that would be saved to the DB. In this case it's all of the saved inputs and groups
			user: {
				// array of saved input groups
				groups: [],
				// array of saved inputs
				inputs: [
					{
						type: 'text',
						reference_name: 'ref1',
						display_label: 'display label',
						default_value: 'default value',
						custom_validation: '.@#AS/',
						tags: [],
						field_group: '',
						extra: {}
					},
					{
						type: 'number',
						reference_name: 'ref2',
						display_label: 'display label',
						default_value: 'default value',
						custom_validation: '.@#AS/',
						tags: [],
						field_group: '',
						extra: {}
					},
					{
						type: 'currency',
						reference_name: 'Reference name thing',
						display_label: 'display label',
						default_value: 'default value',
						custom_validation: '.@#AS/',
						tags: [],
						field_group: '',
						extra: {
							currency: 'USD'
						}
					},
					{
						type: 'date',
						reference_name: 'Reference name thing',
						display_label: 'display label',
						default_value: 'default value',
						custom_validation: '.@#AS/',
						tags: [],
						field_group: '',
						extra: {}
					},
					{
						type: 'select',
						reference_name: 'Reference name thing',
						display_label: 'display label',
						default_value: 'default value',
						custom_validation: '.@#AS/',
						tags: [],
						field_group: '',
						extra: {
							options: [
								{
									name: 'Test1',
									value: 'test'
								},
								{
									name: 'Test 2',
									value: 'test2'
								}
							]
						}
					}
				]
			}
		}
	}
}
</script>