<template>
	<main id="app" class="container">

		<h1>Pomodoro timer</h1>
		<p>Click the start button and when the time is up you will hear a short alarm sound</p>

		<div class="time">
			<span class="minutes">
				{{ minutes | double_digits }}
			</span>
			<span>:</span>
			<span>
				{{ seconds | double_digits }}
			</span>
		</div>

		<div class="buttons">
			<button class="button button--start" v-if="!active" @click="start">Start</button>
			<button class="button button--stop" v-else @click="stop">Stop</button>
			<button class="button button--pause" v-if="active" @click="pause">
				<span v-if="!paused">Pause</span>
				<span v-else>Continue</span>
			</button>
		</div>

		<blockquote cite="https://en.wikipedia.org/wiki/Pomodoro_Technique">The Pomodoro Technique is a time management method developed by Francesco Cirillo in the late 1980s. The technique uses a timer to break down work into intervals, traditionally 25 minutes in length, separated by short breaks.
			<sup><a href="https://en.wikipedia.org/wiki/Pomodoro_Technique" target="_blank">[1]</a></sup>
		</blockquote>

		<audio id="audio" src="assets/alarm.mp3" />

	</main>
</template>

<script>
export default {
	name: 'app',
	data() {
		return {
			minutes: 25,
			seconds: 0,
			active: false,
			paused: false
		}
	},
	methods: {
		start() {

			this.check()
			this.active = true
			this.interval = setInterval(this.check, 1000)

		},
		pause() {

			this.paused = this.paused !== true

		},
		check() {

			if(this.paused)
				return

			this.seconds--

			if (this.seconds < 0) 
				this.seconds = 59
			
			if(this.seconds == 59)  {
				console.log('asdfasdf')
				this.minutes--
			}

			// If finished clear	
			if(this.minutes < 1 && this.seconds < 1) {
				this.stop()
				this.playSound()
			}

		},
		stop() {
			this.minutes = 25
			this.seconds = 0
			this.paused = false
			this.active = false
			clearInterval(this.interval)
		},
		playSound() {
			this.audio.play();
		}
	},
	filters: {
		double_digits(value) {

			if(value.toString().length === 1) 
				return '0' + value.toString()
			else 
				return value

		}
	},
	mounted() {
		this.audio = document.getElementById('audio');
		if(this.active)
			this.start()
	}
}
</script>

<style lang="scss">
body {
	font: 400 1rem "Open Sans";
}
#app {
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}

h1 {
	text-transform: uppercase;
	font: 300 2rem "Open Sans";
}

blockquote {
	font-style: italic;
	max-width: 850px;
	margin: auto;
	margin-bottom: 1rem;
}

.time {
	margin-bottom: 40px;
	margin-top: 40px;
	display: flex;
	align-items: center;
	justify-content: center;
	font: 300 4.5rem "Roboto";
	span {
		display: inline-block;
	}
}

.button {
	font: 300 1.2rem "Roboto";
	color: #fff;
	box-shadow: none;
	border: 0;
	padding: .5em 1em;
	border-radius: .1em;
	margin-bottom: 60px;
	cursor: pointer;
	&--start {
		background: #008000;
	}
	&--pause {
		background: blue;
	}
	&--stop {
		background: #f01;
	}
	&:hover, &:focus, &:active:focus {
		color: #fff;
		outline: none;
	}
}

a {
	color: #42b983;
}
</style>
