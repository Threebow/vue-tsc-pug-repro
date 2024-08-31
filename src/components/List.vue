<script setup lang="ts" generic="T">
	// This file is not generally important for the reproduction. It is simply implemented as an example of a generic
	// component with somewhat complicated prop definitions, that someone may use in their project.

	const props = defineProps<{
		items: T[],
		idKey: keyof T,
		keyToDisplay: keyof T,
		textFactory: (item: T) => string
	}>()

	const model = defineModel<T | null>()
</script>

<template lang="html">
    <p>Selected Item: {{ model ? textFactory(model) : "N/A" }}</p>

    <div class="list">
		<!--
			Another weird thing, but not within the scope of this issue. Using `item[idKey]` instead of
			`item[props.idKey]` introduces a nasty type error that doesn't seem to be intentional. I may
			open a separate issue for this, but in any case, using `props.idKey` works fine.
		-->
        <button v-for="item in items" :key="String(item[props.idKey])" @click="model = item">
            {{ textFactory(item) }}
        </button>
    </div>
</template>