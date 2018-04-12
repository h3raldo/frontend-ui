<template>
	<div class="editor">
		<div class="columns">
			<div class="column col-8 col-md-12 relative">
				<div class="editor_fields_container">
					<h3>Field Details</h3>
					<Default :current="view.current" :inputs="user.inputs" :types="app.input_types" :tags="app.tags"></Default>
					<Extra :current="view.current" @set-extra="$emit('set-extra', $event)"></Extra>
				</div>
			</div>

			<div class="column col-4 col-md-12">
				<Groups :current="view.current" :groups="user.groups" :inputs="user.inputs" :new_group="app.new_group"></Groups>
			</div>
		</div>
	</div>
</template>
<script>
import Default from './Editor/Default.vue'
import Extra from './Editor/Extra.vue'
import Groups from './Editor/Groups.vue'

export default {
	name: 'Editor',
	props: {
		view: Object,
		user: Object,
		app: Object
	},
	components: {
		Default, Extra, Groups
	}
}
</script>
<style>
	.editor{
		height:100%;
	}
	.editor_fields_container{
		padding: 20px 10px;
		position: absolute;
	    height: 100%;
	    width: 100%;
	    overflow-y: scroll;
	}
	.container, .columns{
		height: 100%;
	}
</style>