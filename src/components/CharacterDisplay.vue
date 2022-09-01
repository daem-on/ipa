<script setup>
import { computed } from "@vue/reactivity";
import { defineProps } from "vue";
import data from "../data2.json";
import lists from "../lists.json";
// sources:
// https://en.wikipedia.org/wiki/Help:IPA
// https://www.ling.upenn.edu/courses/Fall_2014/ling115/phonetics.html

const props = defineProps({
	value: {
		type: String,
		default: "",
		required: true,
	}
})

function findCharacter(value) {
	return data.find(character => character.id === value);
}

const result = computed(() => findCharacter(props.value));
</script>

<template>
	<div 
		v-if="lists.spaces.includes(value)"
		class="space">
	</div>
	<div 
		v-else-if="lists.tie.includes(value)"
		class="tie char-display">
	</div>
	<div
		v-else class="char-display"
		:class="{
			'proceeding': lists.proceeding.includes(value),
			'succeeding': lists.succeeding.includes(value)
		}">
		<div class="id">{{ value }}</div>
		<div
			v-if="result != null"
			class="details">
			<div class="value">
				<div v-if="result.value">{{ result.value }}</div>
				<div v-else class="notfound">No value</div>
			</div>
			<div class="example">
				<div v-if="result.example" v-html="result.example"></div>
				<!-- <div v-else class="notfound">No example</div> -->
			</div>
		</div>
		<div v-else class="notfound">
			No entry found
		</div>
	</div>
</template>

<style scoped>
	.char-display {
		display: flex;
		flex-direction: row;
		align-items: center;
		background-color: rgb(255, 249, 242);
		border-radius: 10px;
		margin: 6px 0;
		color: rgb(75, 51, 17);
		border: 1px solid rgb(170, 145, 109);
		padding: 4px;
		padding-right: 12px;
		line-height: 1.5em;
		max-width: 50ch;
		position: relative;
		z-index: 1;
	}

	.succeeding, .proceeding {
		background-color: rgb(255, 243, 228);
	}

	.succeeding {
		margin-top: -12px;
		border-radius: 0 0 10px 10px;
		z-index: -1;
	}
	
	.proceeding {
		margin-bottom: -12px;
		border-radius: 10px 10px 0 0;
	}

	.tie {
		border-radius: 0;
		content: none;
		z-index: 2;
		margin: -13px 0;
		border-top: none;
		border-bottom: none;
	}
	
	.details {
		padding: 5px;
	}

	.space {
		height: 1em;
	}

	.value {
		font-size: 13pt;
	}

	.id {
		font-size: 25pt;
		width: 42px;
		height: 42px;
		margin: 8px;
		display: flex;
		justify-content: center;
		align-items: center;
		border: 1px solid rgb(202, 176, 139);
		border-radius: 5px;
		flex: 0 0 auto;
	}

	.notfound {
		color: rgb(175, 133, 71);
	}
</style>

<style>
	.example i {
		background-color: rgb(255, 226, 190);
		border-radius: 3px;
		padding: 0.1em 0.2em;
		font-style: normal;
	}
</style>