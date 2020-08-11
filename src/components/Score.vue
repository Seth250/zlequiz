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
			<button type="button" class="btn-sec">Play Again</button>
		</div>
	</div>
</template>

<script>
export default {
	name: 'Score',
	props: {
		numCorrectAnswers: Number
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
			console.log(this.canvasWidth, this.canvasHeight)
			this.diff = (this.num / 100) * (Math.PI * 2 * 10)
			this.counterElem.clearRect(0, 0, this.canvasWidth, this.canvasHeight)
			this.counterElem.lineWidth = 18
			this.counterElem.fillStyle = '#000'
			this.counterElem.strokeStyle = 'navy'
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
		// this.fill = setInterval(() => this.fillCounter(this.percentageScore), 40)
		this.fill = setInterval(() => this.fillCounter(80), 40)

		// let diff = 0 // default difference
		// let num = 0 // starting point
		// const pointToFill = 4.72 // point from where the filling starts
		// const counter = document.getElementById('counter').getContext('2d')
		// function refillCounter(value) {
		// diff = (num / 100) * (Math.PI * 2 * 10) // difference between current value(num) and end(100)
		// const canvasWidth = counter.canvas.width
		// const canvasHeight = counter.canvas.height
		// counter.clearRect(0, 0, canvasWidth, canvasHeight) // clear canvas every time when function is called
		// counter.lineWidth = 15 // size of stroke
		// counter.fillStyle = '#000' // color of the counter number
		// counter.strokeStyle = 'yellow' // stroke color
		// counter.textAlign = 'center'
		// counter.font = '500 1.5rem Roboto' // setting font properties
		// counter.fillText(`${num}%`, 100, 110) // fillText(text, x, y)
		// counter.beginPath()
		// counter.arc(100, 100, 90, pointToFill, diff / 10 + pointToFill) // arc(x, y, radius, start, stop)
		// counter.stroke() // fill stroke
		// if (num >= value) {
		// 	clearInterval(fill)
		// }
		// }
		// const fill = setInterval(() => fillCounter(this.percentageScore), 40)
		// const fill = setInterval(() => fillCounter(100), 40)
	}
}
</script>
