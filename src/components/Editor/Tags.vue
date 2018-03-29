<template>
	<div class="editor_tags">
		<div v-for="(tag, index) in this.getTags()">
			<button @click="addTag(tag)">{{ tag.name }}</button>
		</div>
		<div v-for="(tag, index) in current.tags">
			{{ tag.name }} <button @click="removeTag(index)">X</button>
		</div>
	</div>
</template>
<script>
export default {
  name: 'Tags',
  props: {
		current: Object,
		types: Array
  },
	methods: {
		getTags: function()
		{
			let self = this;
			let types = this.types.filter(function( type ) {
				return ( type.code === self.current.type );
			});
			let type,tags;

			if( types.length < 1 ){
				return []
			}

			return types[0].tags.filter(function(tag){
				return !self.isTagAdded( tag.name )
			});

		},
		addTag: function( tag )
		{
			// if it is, don't do anything
			if( this.isTagAdded( tag.name ) ){
				return;
			}

			this.current.tags.push( Object.assign({},tag) );
		},
		removeTag: function( tagIndex )
		{
			this.current.tags.splice(tagIndex, 1);
		},
		isInObjectArray: function( array, name, value ){
			let matched = array.filter(function(item){
				return item.name === value
			});

			return ( matched.length > 0 );
		},
		isTagAdded: function( tagName )
		{
			return this.isInObjectArray( this.current.tags, 'name', tagName );
		}
	}
}
</script>
