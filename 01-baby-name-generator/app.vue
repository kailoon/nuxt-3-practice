<script setup lang="ts">
import { Gender, Popularity, Length, names } from '@/data'


interface OptionsState {
	gender: Gender,
	popularity: Popularity,
	length: Length
}

const options = reactive<OptionsState>({
	gender: Gender.GIRL,
	popularity: Popularity.TRENDY,
	length: Length.LONG
})

const computeSelectedNames = () => {
	const filterNames = names.filter(name => name.gender === options.gender).filter(name => name.popularity === options.popularity).filter(name => {
		if (options.length === Length.ALL) return true
		else return name.length === options.length
	})

	selectedNames.value = filterNames.map(name => name.name)
}

const selectedNames = ref<string[]>([])

const optionsArray = [
	{
		title: "1) Choose a gender",
		category: "gender",
		buttons: [Gender.GIRL, Gender.UNISEX, Gender.BOY],
	},
	{
		title: "2) Choose the name's popularity",
		category: "popularity",
		buttons: [Popularity.TRENDY, Popularity.UNIQUE],
	},
	{
		title: "3) Choose name's length",
		category: "length",
		buttons: [Length.SHORT, Length.ALL, Length.LONG],
	},
];

const removeName = (index: number) => {
	const filterNames = [...selectedNames.value]
	filterNames.splice(index, 1)
	selectedNames.value = filterNames
}

</script>

<template>
	<div class="container">
		<h1>Baby Name Generator</h1>
		<p>Choose your options and click the "Find Names" button below.</p>
		<div class="options-container">

			<Option v-for="option in optionsArray" :key="option.title" :option="option" :options="options" />
			<button class="primary" @click="computeSelectedNames">Find Names</button>
		</div>
		<div class="cards-container">
			<CardName v-for="(name, index) in selectedNames" :key="name" :index="index" :name="name"
				@remove="() => removeName(index)" />
		</div>

	</div>
</template>

<style scoped>
.container {
	font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
		Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
	font-size: 1rem;
	color: #000;
	max-width: 50rem;
	text-align: center;
	margin-inline: auto;
}

h1 {
	font-size: 3rem;
	letter-spacing: -0.44px;
	margin-bottom: 0;
}

.options-container {
	border-radius: 2rem;
	background-color: #b3ffb360;
	padding: 1rem;
	width: 95%;
	position: relative;
	margin-top: 4rem;
}

.primary {
	background-color: rgb(249, 87, 89);
	color: white;
	border: none;
	padding: 1rem;
	width: 12rem;
	font-size: 1rem;
	margin-top: 1rem;
	cursor: pointer;
	border-radius: 1rem;
}

.cards-container {
	display: flex;
	flex-wrap: wrap;
	margin-top: 3rem;
}
</style>
