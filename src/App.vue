<template>
  <div id="app">
		<Header />
		<h2>App</h2>
		<main class="main-content">
			<QuestionBox
				v-if="questions.length && index < 10"
				:currentQuestionObj="questions[index]"
				:nextIndex="nextIndex"
				:questionNumber="questionNumber"
			/>
			<Score
				v-if="index === 10"
				:numCorrectAnswers="numCorrect"
			/>
		</main>
		<Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import Score from './components/Score.vue'
import Footer from './components/Footer.vue'

export default {
	name: 'App',
	components: {
		Header,
		QuestionBox,
		Score,
		Footer
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

<style lang="scss">
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
