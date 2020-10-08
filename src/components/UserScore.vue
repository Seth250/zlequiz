<template>
	<div class="container">
		<div class="score-header">
			<h2>You scored:</h2>
		</div>
		<div class="score-details">
			<div class="circle"></div>
			<canvas id="counter" width="200" height="200"></canvas>
		</div>
		<div class="btn-container">
			<button type="button" class="btn-sec" @click="resetParams">Play Again <i class="fas fa-sync"></i></button>
		</div>
	</div>
</template>

<script>
export default {
	name: 'UserScore',
	props: {
		numCorrectAnswers: Number,
		resetParams: Function
	},
	data() {
		return {
			diff: 0,
			num: 0,
			pointToFill: 4.72,
			fill: null,
			counterElem: null,
			canvasHeight: null,
			canvasWidth: null
		}
	},
	computed: {
		percentageScore() {
			return this.numCorrectAnswers * 10
		}
	},
	methods: {
		fillCounter(value) {
			this.diff = (this.num / 100) * (Math.PI * 2 * 10)
			this.counterElem.clearRect(0, 0, this.canvasWidth, this.canvasHeight)
			this.counterElem.lineWidth = 18
			this.counterElem.fillStyle = '#000'
			// this.counterElem.strokeStyle = '#071f86'
			// this.counterElem.strokeStyle = '#383f77'
			// this.counterElem.strokeStyle = '#3b4c83'
			this.counterElem.strokeStyle = 'rgb(59, 76, 131)'
			this.counterElem.textAlign = 'center'
			this.counterElem.font = '500 1.5rem Roboto'
			this.counterElem.fillText(`${this.num}%`, 100, 110)
			this.counterElem.beginPath()
			this.counterElem.arc(100, 100, 90, this.pointToFill, this.diff / 10 + this.pointToFill)
			this.counterElem.stroke()
			if (this.num >= value) {
				clearInterval(this.fill)
			}
			this.num++
		}
	},
	mounted() {
		this.counterElem = document.getElementById('counter').getContext('2d')
		this.canvasWidth = this.counterElem.canvas.width
		this.canvasHeight = this.counterElem.canvas.height
		this.fill = setInterval(() => this.fillCounter(this.percentageScore), 40)
	}
}
</script>
