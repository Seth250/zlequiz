<template>
	<div class="container">
		<div class="question">
			<h3>Question {{ questionNumber }} / 10</h3>
			<h2>{{ currentQuestion }}</h2>
		</div>
		<div class="answers-container">
			<ul>
				<li
					v-for="(answer, index) in shuffledAnswersArr"
					:key="index"
					class="answer"
				>
					{{ answer }}
				</li>
			</ul>
			<button type="button" @click="nextIndex">{{ nextOrEnd }}</button>
		</div>
	</div>
</template>

<script>
import he from 'he'
import _ from 'lodash'

export default {
	name: 'QuestionBox',
	props: {
		currentQuestionObj: Object,
		nextIndex: Function,
		questionNumber: Number
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
			this.correctIndex = this.shuffledAnswersArr.indexOf(this.currentQuestionObj.correct_answer)
			console.log(this.correctIndex)
		}
	},
	computed: {
		currentQuestion() {
			return he.decode(this.currentQuestionObj.question)
		},
		nextOrEnd() {
			return this.questionNumber < 10 ? 'Next' : 'End'
		}
	}
}
</script>
