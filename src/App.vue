<template>
	<!-- v-if="questions.length && index < 10"
				v-if="index === 10"
											-->
  <div id="app">
		<Header />
		<main class="main-content">
			<QuestionBox
				v-if="questions.length > 10"
				:currentQuestionObj="questions[index]"
				:nextIndex="nextIndex"
				:questionNumber="questionNumber"
				:incrementScore="incrementScore"
			/>
			<Score
				v-if="index < 10"
				:numCorrectAnswers="numCorrect"
			/>
		</main>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import Score from './components/Score.vue'

export default {
	name: 'App',
	components: {
		Header,
		QuestionBox,
		Score
	},
	data() {
		return {
			questions: [],
			index: 0,
			numCorrect: 0
		}
	},
	methods: {
		nextIndex() {
			this.index++
		},
		incrementScore() {
			this.numCorrect++
		}
	},
	computed: {
		questionNumber() {
			return this.index + 1
		}
	},
	async created() {
		try {
			const response = await fetch('https://opentdb.com/api.php?amount=10&type=multiple')
			const { results } = await response.json()
			this.questions = results
		} catch (err) {
			console.log(err)
		}
	}
}
</script>
