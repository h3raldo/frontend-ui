<template>
	<div class="builder_fields">
		<div class="columns">
			<div class="column col-8">
				<h3>Field Details</h3>
				<Default :current="view.current" :types="app.input_types"></Default>
				<Extra :current="view.current" @set-extra="$emit('set-extra', $event)"></Extra>
			</div>

			<div class="column col-4">
				<h3>Field Groups</h3>
				<Groups :current="view.current" :groups="user.groups" :inputs="user.inputs"></Groups>
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
