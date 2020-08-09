<template>
	<div>
		<h2>Question 1 / 10</h2>
		<p>{{ currentQuestion }}</p>
		<p v-for="(answer, index) in shuffledAnswersArr" :key="index">{{ answer }}</p>
	</div>
</template>

<script>
import he from 'he'
import _ from 'lodash'

export default {
	name: 'QuestionBox',
	props: {
		currentQuestionObj: Object
	},
	data() {
		return {
			shuffledAnswersArr: [],
			selectedIndex: null,
			correctIndex: null,
			questionAnswered: false
		}
	},
	watch: {
		currentQuestionObj: {
			immediate: true,
			handler() {
				this.selectedIndex = null
				this.shuffleAnswers()
				this.questionAnswered = false
			}
		}
	},
	methods: {
		shuffleAnswers() {
			const answers = [he.decode(this.currentQuestionObj.correct_answer)]
			this.currentQuestionObj.incorrect_answers.forEach((answer) => answers.push(he.decode(answer)))
			this.shuffledAnswersArr = _.shuffle(answers)
			this.correctIndex = this.shuffleAnswers.indexOf(this.currentQuestionObj.correct_answer)
		}
	},
	computed: {
		currentQuestion() {
			return he.decode(this.currentQuestionObj.question)
		}
	}
}
</script>
