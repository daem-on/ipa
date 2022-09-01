<script setup>
import { computed } from "@vue/reactivity";
import { ref } from "vue";
// import GraphemeSplitter from "grapheme-splitter";
import CharacterDisplay from "./CharacterDisplay.vue";
import DiagonalDisplay from "./DiagonalDisplay.vue";

const text = ref("");
const overview = ref(true);
const normalized = computed(() => text.value.normalize())
</script>

<template>
	<div class="main" :class="{'fullheight': overview}">
		<div class="header">
			<input type="text" v-model="text"
				placeholder="Enter IPA here">
			<button @click="overview = !overview" class="button">
				<span v-if="overview">Overview</span>
				<span v-else>Details</span>
			</button>
		</div>
		<div v-if="text.length !== normalized.length">
			<br>
			This text needed to be normalized.
		</div>
		<div :class="{'row': overview}">
			<component
				:is="overview ? DiagonalDisplay : CharacterDisplay"
				:value="character"
				v-for="(character, index) of normalized"
				:key="character+index">
			</component>
		</div>
	</div>
</template>

<style>
	.main {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	.header {
		display: flex;
		flex-direction: row;
		align-items: center;
		width: 100%;
		justify-content: center;
	}

	.header .button {
		font-size: 13pt;
		font-family: inherit;
		margin: 16px;
		background-color: rgb(255, 249, 242);
		border-radius: 10px;
		padding: 8px 4px;
		cursor: default;
		width: 10ch;
		text-align: center;
		color: rgb(75, 51, 17);
		border: 1px solid rgb(170, 145, 109);
	}

	body {
		margin: 0;
	}

	html, body, #app, .fullheight {
		height: 100%;
	}

	.row {
		display: flex;
		flex-direction: row;
		width: 100%;
		flex: 1;
		overflow-x: scroll;
		overflow-y: hidden;
		position: relative;
	}

	.main input {
		height: 1.8em;
		line-height: 2;
		margin: 0.6em;
		border: none;
		font-size: 25pt;
		text-align: center;
		border-radius: 5px;
		background-color: rgb(253, 215, 169);
		border: 1px solid rgb(170, 145, 109);
		font-family: serif;
	}

	.main input::placeholder {
		color: rgb(175, 133, 71);
	}

	html {
		font-size: 15pt;
		background-color: blanchedalmond;
	}
</style>