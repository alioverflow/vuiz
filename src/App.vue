<script setup>
import { ref, computed } from 'vue'

const quizQuestions = ref([
  {
	question: 'Vue',
	answer: 1,
	options: [
		'a monolithic JS library for building user interfaces',
		'a framework for building user interfaces',
	],
	selected: null
  },
  {
	question: 'Vuex',
	answer: 1,
	options: [

		'a design pattern for Vue',
		'a state management pattern and library'
	],
	selected: null
  },
  {
	question: 'Vue Router',
	answer: 0,
	options: [
		'the official router for Vue.js',
		'a Vue library for API requests'
	],
	selected: null
  },
   {
	question: 'Vite',
	answer: 0,
	options: [
		'a tool to set up a development environment',
		'a backend service for vue applications'
	],
	selected: null
  },
  
  {
	question: 'Vue CLI',
	answer: 1,
	options: [
		'the controller layer in vue applications',
		'control vue with commands in your terminal'
	],
	selected: null
  },
  {
	question: 'v-bind',
	answer: 0,
	options: [
		'a directive used to bind one or more attributes',
		'a library to build mobile apps using vue'
	],
	selected: null
  }
])


const isQuizCompleted = ref(false) , currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	quizQuestions.value.map(q => {
		if (q.selected != null && q.answer == q.selected)
			value++
	})
	return parseInt(value*100/6)
})

const getCurrentQuestion = computed(() => {
	let question = quizQuestions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const setAnswer = (e) => {
	quizQuestions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const nextQuestion = () => {
	if (currentQuestion.value < quizQuestions.value.length - 1) {
		currentQuestion.value++
		return
	}
	isQuizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1>V U I Z</h1>
		<section class="quiz" v-if="!isQuizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score: %{{ score }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="setAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="nextQuestion" 
				:disabled="!getCurrentQuestion.selected">
				{{ 
					getCurrentQuestion.index == quizQuestions.length - 1 
						? 'Finish !' 
						: getCurrentQuestion.selected == null
							? 'Select your option'
							: 'Next'
				}}
			</button>
		</section>

		<section v-else>
			<p>Your score: %{{ score }}</p>
		</section>
	</main>
</template>

<style>
@import '../assets/styles/main.css';
</style>