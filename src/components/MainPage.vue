<script setup>
import { computed } from "@vue/reactivity";
import { ref } from "vue";
// import GraphemeSplitter from "grapheme-splitter";
// import CharacterDisplay from "./CharacterDisplay.vue";
import DiagonalDisplay from "./DiagonalDisplay.vue";

const text = ref("");
const normalized = computed(() => text.value.normalize())
</script>

<template>
	<div class="main">
		<div>
			<input type="text" v-model="text">
		</div>
		<div v-if="text.length !== normalized.length">
			<br>
			This text needed to be normalized.
		</div>
		<div class="row">
			<DiagonalDisplay
				:value="character"
				v-for="(character, index) of normalized"
				:key="character+index">
			</DiagonalDisplay>
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

	body {
		margin: 0;
	}

	html, body, #app, .main {
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
		width: 100%;
		height: 1.8em;
		line-height: 2;
		margin: 0.6em 0;
		border: none;
		font-size: 25pt;
		text-align: center;
		border-radius: 5px;
		background-color: rgb(253, 215, 169);
		border: 1px solid rgb(170, 145, 109);
		font-family: serif;
	}

	html {
		font-size: 15pt;
		background-color: blanchedalmond;
	}
</style>