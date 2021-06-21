<template>
  <div class="options" >
    <div :class="`options__status ${isGame?'':'options__status_game-over'}`">{{ status }}</div>
    <div :class="`${listenUser?'options__listen':'options__repeat'}`">
      {{ isGame ? listenUser ? 'Press' : 'Listen' : '' }}
    </div>
    <div class="options__level">
      <div
        v-for="lev in $options.levels"
        :key="lev.index"
        @click="level = lev.wait"
      >
        <input class="options__input" type="radio" :value="lev.wait" v-model="level">
        <label>{{ lev.title }}</label>
      </div>
    </div>
    <div class="options__start-button">
      <button :disabled="isGame" @click="$emit('start-game')">Start!</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Option',
  props: {
    listenUser: Boolean,
    isGame: Boolean,
    level: Number,
    status: String,
  },
  levels: [
    { title: 'Easy', wait: 1500 },
    { title: 'Medium', wait: 1000 },
    { title: 'Hard', wait: 400 }
  ]
}
</script>

<style lang="sass" >


.options
	width: 10rem

	&__level,
	&__start-button
		width: fit-content
		margin: 1rem auto

	&__input
		margin-bottom: 1rem

	&__status
		color: blue
		text-align: center
		font-size: 1.3rem
		height: 2rem
		margin: 0.5rem 0

		&_game-over
			color: red

	&__listen,
	&__repeat
		text-align: center
		font-size: 1rem
		height: 1.5rem

	&__listen
		color: lightgreen

	&__repeat
		color: red

button
  padding: 0.5rem 2.5rem
  border-radius: 0.5rem
  background-color: blue
  color: white

</style>
