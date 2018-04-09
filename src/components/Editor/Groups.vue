<template>
	<div class="editor_groups">
		<div class="editor_groups_fields">
			<h3>Field Groups</h3>
			<div v-for="(group, index) in groups">
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
			this.current.field_group = group;
		},
		removeGroupFromCurrent: function()
		{
			this.current.field_group = '';
		},
		getInputsByGroup( group_name )
		{
			return this.inputs.filter(function(input){
				return input.field_group.name === group_name
			});
		},
		getGroupInputCount: function( group_name )
		{
			let matched = this.getInputsByGroup( group_name );
			return matched.length;
		}

	}
}
</script>
<style>
.editor_groups{
	padding: 20px 10px;
	height: 100%;
	display: flex;
	flex-direction: column;
	overflow-y: scroll;
}
.editor_groups_fields{
	background: #F9FBFB;
	padding: 20px;
	flex: 1;
}
.editor_groups_buttons{
	background: #F9FBFB;
	padding: 0 20px 20px 20px;
}
</style>
