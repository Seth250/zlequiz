<template>
	<div>
		<h2>Your score is {{ percentageScore(numCorrectAnswers) }}%</h2>
	</div>
</template>

<script>
export default {
	name: 'Score',
	props: {
		numCorrectAnswers: Number
	},
	methods: {
		percentageScore(score) {
			return (this.numCorrectAnswers / 10) * 100
		}
	}
}
</script>
