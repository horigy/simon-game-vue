<template>
  <div
    class="color-area"
    @click.prevent="clickColorArea()"
    :style="`background-color:${color}`"
  ></div>
</template>

<script>
export default {
  name: "ColorArea",
  props: {
    areaID: Number,
    color: String,
    listenUser: Boolean
  },
  methods: {
		clickColorArea() {
			if (!this.listenUser) { return }
      this.$emit('clickColorArea', this.areaID )
      this.pushColorArea()
    },
    pushColorArea() {
      this.$el.classList.add('color-area_active')
      this.playSound()
			setTimeout(() => {
				this.$el.classList.remove('color-area_active')
			}, 200)
    },
    playSound() {
			const audio = new Audio(`sound/${this.areaID}.mp3`)
			audio.play()
    }
  }
}
</script>

<style lang="sass">



.color-area
  display: inline-block
  opacity: 0.5

  &_active
    opacity: 1

</style>
