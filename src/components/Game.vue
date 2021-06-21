<template>
	<div class="wrapper">
		<div class="game">
			<ColorArea
				ref="colorArea"
				v-for="(color, index) in colors"
				:key="index"
				:areaID="index + 1"
				:listenUser="listenUser"
				:color="color"
				:class="`game__area`"
				@clickColorArea="checkChain"
			/>
		</div>
		<Options
			:listenUser="listenUser"
			:isGame="isGame"
			:status="status"
			:level="level"
			@start-game="startGame"
		/>
	</div>
</template>

<script>
import ColorArea from './ColorArea'
import Options from './Options'

export default {
	name: 'Game',
	components: {
		ColorArea,
		Options
	},
  data() {
    return {
			isGame: false,
			chain: [],
			status: 'Let`s start?',
			listenUser: false,
			currentStep: 0,
			timer: {},
			round: 1,
			optObj: {
				level: 1500
			},
			level: 1500
    }
	},
  methods: {
		startGame() {
			this.isGame = true
			this.chain = []
			this.round = 1
			this.nextRound()
		},
		nextRound() {
			this.status = `Round ${this.round}`
			this.currentStep = 0
			this.addRandomNumber()
			this.listenUser = false
			this.playChain(this.chain)
		},
		stopGame(status) {
			this.isGame = false
			this.status = status
			this.chain = []
			this.currentStep = 0
			this.listenUser = false
		},
		checkChain(val) {
			if (!this.listenUser) { return false }
			clearTimeout(this.timer)
			if (this.chain[this.currentStep] !== val) {
				this.stopGame('Failure')
				return false
			}
			if (this.chain.length - 1 === this.currentStep) {
				this.round++
				this.timer = setTimeout(() => this.nextRound(),
					this.level < 1000 ? 1000 : this.level)
				return false
			}
			this.currentStep++
			this.timer = setTimeout(() => this.stopGame('Timeout'), this.level)
		},
		playChain(chain) {
			if (!chain.length) {
				this.listenUser = true
				return
			}
			setTimeout(() => {
				this.playChain(chain.slice(1))
			}, this.level > 700 ? 700 : this.level)
			this.$refs.colorArea[chain[0]-1].pushColorArea()
		},
		addRandomNumber() {
			this.chain.push(this.randomInteger(1, 4))
		},
		randomInteger(min, max) {
			let rand = min + Math.random() * (max + 1 - min);
			return Math.floor(rand);
		}
	},
	computed: {
		colors() {
			return ['blue', 'red', 'green', 'orange']
		}
	}
}
</script>

<style lang="sass" >

.game
	border-radius: 9rem
	border: 0
	overflow: hidden
	display: flex
	flex-wrap: wrap
	width: 18rem

	&__area
		width: 9rem
		height: 9rem

</style>
