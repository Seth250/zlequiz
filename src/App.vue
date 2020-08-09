<template>
  <div id="app">
		<Header />
		<h2>App</h2>
		<main class="main-content">
			<QuestionBox
				v-if="questions.length"
				:currentQuestionObj="questions[index]"
			/>
		</main>
		<Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
import Footer from './components/Footer.vue'

export default {
	name: 'App',
	components: {
		Header,
		QuestionBox,
		Footer
	},
	data() {
		return {
			questions: [],
			index: 0,
			numCorrect: 0
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
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
