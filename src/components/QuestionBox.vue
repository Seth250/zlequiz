<template>
	<div class="container">
		<div class="question">
			<h3>Question {{ questionNumber }} / 10</h3>
			<div class="progress-bar" :style="progressStyle"></div>
			<h2>{{ currentQuestion }}</h2>
		</div>
		<div class="answers-container">
			<ul>
				<li
					v-for="(answer, index) in shuffledAnswersArr"
					:key="index"
					tabindex="0"
					class="answer"
					:class="[answerClass(index)]"
					@click="selectAnswer(index)">
					{{ answer }}
				</li>
			</ul>
			<button
				type="button"
				@click="nextIndex"
				class="btn-pry"
				:class="{disabled: !questionAnswered}"
				:disabled="!questionAnswered">
				{{ nextOrEndText }}
			</button>
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
		questionNumber: Number,
		incrementScore: Function
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
			const correctAnswer = he.decode(this.currentQuestionObj.correct_answer)
			const answers = [correctAnswer]
			this.currentQuestionObj.incorrect_answers.forEach((answer) => answers.push(he.decode(answer)))
			this.shuffledAnswersArr = _.shuffle(answers)
			this.correctIndex = this.shuffledAnswersArr.indexOf(correctAnswer)
			// console.log(this.correctIndex)
		},
		selectAnswer(index) {
			this.selectedIndex = index
			this.questionAnswered = true
			if (this.selectedIndex === this.correctIndex) {
				this.incrementScore()
			}
		},
		answerClass(index) {
			if (this.questionAnswered) {
				let answerClass = 'disabled '
				if (this.correctIndex === index) {
					answerClass += 'correct-answer'
				} else if (this.selectedIndex !== this.correctIndex && this.selectedIndex === index) {
					answerClass += 'incorrect-answer'
				}
				return answerClass
			}
		}
	},
	computed: {
		currentQuestion() {
			return he.decode(this.currentQuestionObj.question)
		},
		nextOrEndText() {
			return this.questionNumber < 10 ? 'Next' : 'End Quiz'
		},
		progressStyle() {
			// return `width: ${this.questionNumber / 10 * 100}%;`
			return `transform: scaleX(${this.questionNumber / 10})`
		}
	}
}
</script>
