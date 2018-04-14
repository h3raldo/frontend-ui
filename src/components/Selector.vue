<template>
	<div class="selector">
		<h3 class="mb-2">Field Types</h3>

		<div class="selector_fields_show" v-bind:class="{ 'd-none': open }">
			<p><button class="btn btn-default" @click="showTypes()">Select Field Type</button></p>
		</div>

		<div class="selector_fields" v-bind:class="{ 'open': open }">

			<div class="form-group mb-2">
				<label class="form-label">Filter Types</label>
				<input class="form-input" type="text" placeholder="Filter" v-model="filter_term">
			</div>

			<template v-for="type in availableTypes()">
				<div class="card" 
					:key="type.code" 
					@click="$emit('new-input', type.code); hideTypes()" 
					v-bind:class="{ 'bg-primary': type.code === current.type }">

					<div class="card-header">
						<div class="card-title h5">{{ type.name }}</div>
						
					</div>
					<div class="card-body">
						<div class="card-subtitle text-gray">Definition</div>
						<p>{{ type.description }}</p>

						<div class="card-subtitle text-gray">Default Display</div>
						<p>{{ type.default_display }}</p>
					</div>
				</div>
			</template>

		</div>
	</div>
</template>

<script>
export default {
	name: 'Selector',
	props: {
		types: Array,
		current: Object
	},
	data: function(){
		return{
			filter_term: '',
			open: false
		}
	},
	methods: {
		/**
		 * Opens the selector types
		*/
		showTypes: function()
		{
			this.open = true;
		},
		/**
		 * Hides the selector types
		*/
		hideTypes: function()
		{
			this.open = false;
		},
		/**
		 * Returns available field types based on entered search term,
		 * returns all types if no search term is entered
		 *
		 * @returns {array}
		 */
		availableTypes: function()
		{
			// return full array if no search term has been entered
			if( this.filter_term.length < 1 ) return this.types;

			let self = this;
			return this.types.filter(function( type ){
				return type.name.toLowerCase().includes( self.filter_term.toLowerCase() );
			});
		}
	}
}
</script>
