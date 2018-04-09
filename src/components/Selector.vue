<template>
	<div class="selector">
		<h3 class="mb-2">Field Types</h3>


		<div class="form-group mb-2">
			<label class="form-label">Filter Types</label>
			<input class="form-input" type="text" placeholder="Filter" v-model="filter_term">
		</div>

		<template  v-for="type in availableTypes()">
			<div class="card" 
				v-on:click="$emit('new-input', type.code)" 
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
			filter_term: ''
		}
	},
	methods: {
		availableTypes: function(){
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
<style>
	.selector{
		position: absolute;
		height: 100%;
		width: 100%;
		overflow-y: scroll;
		background: #EEF4F5;
		padding: 20px;
		padding-bottom: 0;
	}
	.card{
		margin-bottom: 20px;
	}
</style>
