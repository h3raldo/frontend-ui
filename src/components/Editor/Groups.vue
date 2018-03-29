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
				<h3>{{ group.name }}</h3>
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
		inputs: Array
  },
	data: function(){
		return {
			new_group: {
				name: "",
				edit: true
			}
		}
	},
	methods: {
		addGroup: function(){
			let new_group = Object.assign({}, this.new_group);
			this.groups.push( new_group );
		},
		saveGroup: function( index ){
			this.groups[index].edit = false;
		},
		editGroup: function( index ){
			this.groups[index].edit = true;
		}
	}
}
</script>
