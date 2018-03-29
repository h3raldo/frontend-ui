<template>
	<div class="editor_groups">
		<div v-for="(group, index) in groups">
			<template v-if="group.edit === true">
				<input type="text" v-model="group.name" placeholder="Enter Name">
				<template v-if="group.name.length > 0">
					<button @click="saveGroup(index)">Save</button>
				</template>
			</template>
			<template v-else>
				<h3>{{ group.name }} - {{ getGroupInputCount(group.name) }}</h3>
				<button v-if="group.name === current.field_group" @click="removeGroupFromCurrent(group)">Unassign</button>
				<button v-else @click="addGroupToCurrent(group)">Assign to Input</button>
				<button @click="editGroup(index)">Edit</button>
			</template>
		</div>
		<button @click="addGroup()">Add Group</button>
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
		addGroup: function()
		{
			let new_group = Object.assign({}, this.new_group);
			this.groups.push( new_group );
		},
		saveGroup: function( index )
		{
			this.groups[index].edit = false;
		},
		editGroup: function( index )
		{
			this.groups[index].edit = true;
		},
		addGroupToCurrent: function( group )
		{
			this.current.field_group = group.name;
		},
		removeGroupFromCurrent: function()
		{
			this.current.field_group = '';
		},
		getInputsByGroup( group_name )
		{
			return this.inputs.filter(function(input){
				return input.field_group === group_name
			});
		},
		getGroupInputCount: function( group_name )
		{
			let matched = this.getInputsByGroup(group_name);
			return matched.length;
		}

	}
}
</script>
