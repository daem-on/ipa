<script setup>
	import { computed } from "@vue/reactivity";
	import { defineProps } from "vue";
	import data from "../data2.json";
	import lists from "../lists.json";

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
		class="tie id">
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
			<div class="example" v-if="result.example" v-html="result.example"></div>
		</div>
		<div v-else class="notfound">
			No entry found
		</div>
	</div>
</template>

<style>
	.char-display {
		display: flex;
		flex-direction: row;
		margin: 5px;
	}

	.char-display:first-child, .space:first-child {
		margin-left: 1em;
	}
	
	.succeeding {
		margin-left: -12px;
		border-radius: 10px 0;
		z-index: -1;
	}
	
	.proceeding {
		margin-right: -12px;
		border-radius: 10px 10px 0 0;
	}

	.succeeding .id, .proceeding .id {
		background-color: rgb(255, 243, 228);
	}

	.tie.id {
		border-radius: 0;
		content: none;
		z-index: 2;
		width: 15px;
		margin: 5px -13px;
		border-left: none;
		border-right: none;
	}
	
	.details {
		margin-left: 40px;
		margin-top: 50px;
		transform: rotate(+45deg);
		transform-origin: 0 0;
		position: absolute;
		white-space: nowrap;
		text-overflow: ellipsis;
		background-color: rgb(255, 249, 242);
		padding: 0.1em 0.2em;
		border-radius: 5px;
		color: rgb(175, 133, 71);
	}

	.details:empty {
		display: none;
	}

	.example i {
		/* background-color: rgb(255, 226, 190);
		border-radius: 3px;
		padding: 0.1em 0.2em; */
		font-style: normal;
		color: rgb(20, 17, 13);
	}

	.id {
		font-size: 25pt;
		background-color: rgb(255, 249, 242);
		width: 42px;
		height: 42px;
		display: flex;
		justify-content: center;
		align-items: center;
		border: 1px solid rgb(202, 176, 139);
		border-radius: 5px;
	}
</style>