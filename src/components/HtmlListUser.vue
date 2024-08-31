<script setup lang="ts">
	import List from "./List.vue"
	import { TEST_USERS, type User } from "../fixtures"
	import { ref } from "vue"

	const user = ref<User | null>(null)

	declare type Union = {
		tag: "a"
		a: number
	} | {
		tag: "b"
		b: string
	}

	const union: Union = {
		tag: "a",
		a: 123
	} as Union
</script>

<template lang="html">
    <!-- HTML template: works perfectly fine -->

    <!-- This works perfectly fine -->
    <List v-model="user"
          :items="TEST_USERS"
          id-key="id"
          key-to-display="name"
          :text-factory="u => `(${u.id}) ${u.name}`"
    ></List>

    <!-- Changing the `id-key` and `key-to-display` params will error, which is exactly what we want
         This is left commented so the app will compile, uncomment to see it's working as expected -->

    <!-- <List v-model="user"
          :items="TEST_USERS"
          id-key="id_WRONG"
          key-to-display="name_BAD"
          :text-factory="12345678"
    ></List> -->

    <!-- This also works perfectly fine, the types get narrowed as expected -->
    <div v-if="union.tag === 'a'">{{ union.a }}</div>
    <div v-else>{{ union.b }}</div>

    <!-- The inverse of lines 30-31, note that this does not work, which is exactly what we want -->
    <!-- This is also commented so the app will compile. Uncomment to see it's working as expected. -->
    
    <!-- <div v-if="union.tag === 'a'">{{ union.b }}</div>
    <div v-else>{{ union.a }}</div> -->
</template>