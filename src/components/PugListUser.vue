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

<template lang="pug">
	<!-- Exact same HTML template as HtmlUserList.vue, but copied to this file and converted to pug: entirely broken -->
	<!-- Note that in pug files, we also get an `unexpected token: "indent"` error -->

	<!-- This works perfectly fine, which is expected -->
	List(
		v-model="user",
		:items="TEST_USERS",
		id-key="id",
		key-to-display="name",
		:text-factory="u => `(${u.id}) ${u.name}`"
	)

	<!-- This reports zero errors from IDE typecheck or from vue-tsc, as if it never even saw this code -->
	<!-- Note that this does not need to be commented for the application to compile, unlike the HTML component -->
	List(
		v-model="user",
		:items="TEST_USERS",
		id-key="id_WRONG",
		key-to-display="name_BAD",
		:text-factory="12345678"
	)

	<!-- This works perfectly fine but IDE does not show type narrowing hints -->
	div(v-if="union.tag === 'a'") {{ union.a }}
	div(v-else) {{ union.b }}

	<!-- This reports zero errors from IDE typecheck or from vue-tsc, as if it never even saw this code -->
	<!-- Despite this code being clearly incorrect, an access to `b` from our `a`-tagged union not be possible -->
	<!-- Note that this does not need to be commented for the application to compile, unlike the HTML component -->
	div(v-if="union.tag === 'a'") {{ union.b }}
	div(v-else) {{ union.a }}
</template>