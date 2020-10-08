<template>
  <div id="app">
		<PageHeader />
		<main class="main-content">
			<QuestionBox
				v-if="questions.length && index < 10"
				:current-question-obj="questions[index]"
				:next-index="nextIndex"
				:question-number="questionNumber"
				:increment-score="incrementScore"
			/>
			<UserScore
				v-if="index === 10"
				:num-correct-answers="numCorrect"
				:reset-params="resetParams"
			/>
		</main>
  </div>
</template>

<script>
import PageHeader from './components/PageHeader.vue'
import QuestionBox from './components/QuestionBox.vue'
import UserScore from './components/UserScore.vue'

export default {
	name: 'App',
	components: {
		PageHeader,
		QuestionBox,
		UserScore
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
		},
		async fetchData() {
			try {
				const response = await fetch('https://opentdb.com/api.php?amount=10&type=multiple')
				const { results } = await response.json()
				this.questions = results
			} catch (err) {
				console.log(err)
			}
		},
		resetParams() {
			Object.assign(this.$data, this.$options.data.call(this))
			this.fetchData()
		}
	},
	computed: {
		questionNumber() {
			return this.index + 1
		}
	},
	created() {
		this.fetchData()
	}
}
</script>
