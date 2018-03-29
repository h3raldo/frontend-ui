<template>
	<div class="editor_extra">
		<!-- extra fields that are not required for all inputs -->
		<!-- type: currency -->
		<Currency
			v-if='current.type === "currency"'
			:current="current"
			@set-extra="$emit('set-extra', $event)">
		</Currency>

		<!-- type: select -->
		<Select
			v-else-if='current.type === "select"'
			:current="current"
			@set-extra="$emit('set-extra', $event)">
		</Select>
	</div>
</template>
<script>
import Select from './Extra/Select.vue'
import Currency from './Extra/Currency.vue'
export default {
  name: 'Extra',
  props: {
		current: Object
  },
	components: {
		Select, Currency
	}
}
</script>
