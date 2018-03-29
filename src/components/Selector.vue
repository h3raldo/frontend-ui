<template>
	<div class="builder_selector">
		<h2>Field Types</h2>

		<div v-for="type in types">
			<button v-on:click="$emit('new-input', type.code)">New {{ type.name }} Input</button>
		</div>
	</div>
</template>

<script>
export default {
  name: 'Selector',
  props: {
    types: Array
  }
}
</script>
