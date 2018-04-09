<template>
	<div class="editor_tags">
		<h5>Tags</h5>
		<div class="columns">
			<div class="column col-6">
				<p class="mb-1">Tag Group</p>
				<template v-for="(tag) in this.getTags()">
					<span class="label label-primary label-rounded" @click="addTag(tag)">{{ tag.name }}</span>&nbsp;
				</template>
			</div>
			<div class="column col-6">
				<p>Tags</p>
				<template v-if="current.tags">
					<template v-if="getAppliedTags().length < 1">
						<p><i>Select a tag group to see individual tags.</i></p>
					</template>
					<div v-for="(tag, index) in getAppliedTags()">
						<div class="card">
							<div class="card-body">
								<div>
									{{ tag.name }} <button class="btn btn-error btn-sm" @click="removeTag(index)">X</button>
								</div>
								
								<div class="columns">
									<div class="column col-6">
										<b>requires:</b>
										<div v-for="tag in tag.requires">
											{{ tag.name }}<br>
										</div>
									</div>
									<div class="column col-6">
										<b>returns:</b>
										<div v-for="tag in tag.returns">
											{{ tag.name }}<br>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</template>
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
		/**
		 * Get tags currently applied to current input
		 *
		 * @returns {array}
		 */
		getAppliedTags: function(){
			let tags = [];
			let self = this;
			this.current.tags.forEach(function(tagId){
				tags.push( self.getTagById( tagId ) )
			});
			return tags;
		},
		/**
		 * Returns full tag object using ID
		 *
		 * @param {string} id ID of the tag
		 * @returns {object}
		 */
		getTagById: function( id ){
			let tag = this.tags.filter( function(tag){
				return tag.id === id;
			});

			if( tag.length < 1 ){
				return [];
			}

			return tag[0];
		},
		/**
		 * Returns available tags based on the input type, 
		 * excludes the applied tags
		 *
		 * @returns {array}
		 */
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
				return !self.isTagAdded( tag.id )
			});

		},
		/**
		 * Adds tag to current input
		 *
		 * @param {object} tag Tag object to be added
		 */
		addTag: function( tag )
		{
			// if it is, don't do anything
			if( this.isTagAdded( tag.name ) ){
				return;
			}

			this.current.tags.push( tag.id ) ;
		},
		/**
		 * Removes tag from current input
		 *
		 * @param {object} tag Tag object to be added
		 */
		removeTag: function( tagIndex )
		{
			this.current.tags.splice(tagIndex, 1);
		},
		/**
		 * Checks if a key-value pair exists in an array of objects
		 *
		 * @param {array} array Array to search
		 * @param {string} name Property name to search
		 * @param {string} value Value the property should equal to
		 */
		isInObjectArray: function( array, name, value ){
			let matched = array.filter(function(item){
				return item.name === value
			});

			return ( matched.length > 0 );
		},
		/**
		 * Checks if tag is already added to current input
		 *
		 * @param {string} tagName Name of tag to search with
		 */
		isTagAdded: function( tagName )
		{
			return this.current.tags.indexOf( tagName ) !== -1;
		}
	}
}
</script>
<style lang="scss" scoped>
	.label{
		cursor: pointer;
	}
</style>
