<template>
	<div class="editor_tags">
		<div v-for="(tag) in this.getTags()">
			<button @click="addTag(tag)">{{ tag.name }}</button>
		</div>
		<div v-for="(tag, index) in getAppliedTags()">
			{{ tag.name }} <button @click="removeTag(index)">X</button>
			<div v-for="tag in tag.requires">
				{{ tag.name }}<br>
			</div>
			<div v-for="tag in tag.returns">
				{{ tag.name }}<br>
			</div>
		</div>
	</div>
</template>
<script>
export default {
	name: 'Tags',
	props: {
		current: Object,
		tags: Array
	},
	methods: {
		getAppliedTags: function(){
			let tags = [];
			let self = this;
			this.current.tags.forEach(function(tagId){
				tags.push( self.getTagById( tagId ) )
			});
			return tags;
		},
		getTagById: function( id ){
			let tag = this.tags.filter( function(tag){
				return tag.id === id;
			});

			if( tag.length < 1 ){
				return [];
			}

			return tag[0];
		},
		getTags: function()
		{
			let self = this;
			let tags = this.tags.filter(function( tag ) {
				return ( tag.types.indexOf( self.current.type ) !== -1 );
			});

			if( tags.length < 1 ){
				return []
			}

			return tags.filter(function(tag){
				return !self.isTagAdded( tag.name )
			});

		},
		addTag: function( tag )
		{
			// if it is, don't do anything
			if( this.isTagAdded( tag.name ) ){
				return;
			}

			this.current.tags.push( tag.id ) ;
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
			return this.current.tags.indexOf( tagName ) !== -1;
		}
	}
}
</script>
