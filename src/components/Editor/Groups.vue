<template>
	<div class="editor_groups">
		<div class="editor_groups_fields">
			<h3>Field Groups</h3>
			<div v-for="(group, index) in groups" :key="group.id">
				<div class="card" 
					v-bind:class="{ 'bg-primary': group.name === current.field_group.name }"
					>
					<template v-if="group.edit === true">
						<div class="input-group">
							<input class="form-input" type="text" v-model="group.name" placeholder="Enter Name">
							<template v-if="group.name.length > 0">
								<button class="btn btn-primary input-group-btn" @click="saveGroup(index)">Save</button>
							</template>
						</div>
					</template>
					<template v-else>
						<div class="card-header" @click="addGroupToCurrent(group)">
							<div class="card-title h5">
								{{ group.name }}
								<button class="btn btn-secondary btn-sm" @click="editGroup(index)"><i class="icon icon-edit"></i></button>
							</div>
							<div class="card-subtitle text-gray">{{ getGroupInputCount(group.name) }} other inputs</div>
						</div>
					</template>
				</div>
			</div>
		</div>
		<div class="editor_groups_buttons">
			<button class="btn btn-secondary btn-block" @click="addGroup()"><i class="icon icon-plus"></i> Add New Group</button>
		</div>
	</div>
</template>
<script>
export default {
	name: 'Groups',
	props: {
		current: Object,
		groups: Array,
		inputs: Array,
		new_group: Object
	},
	methods: {
		/**
		 * Adds a blank group to the user's available groups (global)
		 */
		addGroup: function()
		{
			let new_group = Object.assign({}, this.new_group);
			this.groups.push( new_group );
		},
		/**
		 * Sets the group's edit status to false, disallowing
		 * the user to edit the group
		 *
		 * @param {int} index Index of group to save
		 */
		saveGroup: function( index )
		{
			this.groups[index].edit = false;
		},
		/**
		 * Sets the group's editable status to true, allowing
		 * the user the edit the group
		 *
		 * @param {int} index Index of the group to edit
		 */
		editGroup: function( index )
		{
			this.groups[index].edit = true;
		},
		/**
		 * Applies the group to the current input being edited
		 *
		 * @param {object} group Group object to be added
		 */
		addGroupToCurrent: function( group )
		{
			this.current.field_group = group;
		},
		/**
		 * Removed the currently applied group of the current input
		 */
		removeGroupFromCurrent: function()
		{
			this.current.field_group = '';
		},
		/**
		 * Filters all current user inputs by group name
		 *
		 * @param {string} group_name Name of the group
		 * @returns {array}
		 */
		getInputsByGroup( group_name )
		{
			return this.inputs.filter(function(input){
				return input.field_group.name === group_name
			});
		},
		/**
		 * Counts the amount of inputs that belong to searched 
		 * group name
		 *
		 * @param {string} group_name Name of the group to filter by
		 * @returns {int}
		 */
		getGroupInputCount: function( group_name )
		{
			let matched = this.getInputsByGroup( group_name );
			return matched.length;
		}

	}
}
</script>
