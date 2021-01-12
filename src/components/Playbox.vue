<template>
  <div class="h-48 bg-gray-200">
    <div class="p-3 h-full flex flex-col justify-between">
      <div class="flex justify-between">
        <div class="flex flex-col w-4/5">
          <div class="w-full text-gray-800 text-sm">
            {{ playingEpisode?.podcast?.meta?.title }}
          </div>
          <div class="w-full whitespace-no-wrap overflow-x-hidden">
            <marquee scrollamount="4">
              {{ playingEpisode?.title }}
            </marquee>
          </div>
        </div>
        <div class="flex justify-end items-start w-1/5">
          <font-awesome-icon 
            icon="chevron-down"
            class="text-4xl"
          />
        </div>
      </div>

      <div class="flex justify-between">
        <font-awesome-icon
          icon="step-backward" 
          class="text-4xl"
        />
        <font-awesome-icon
          icon="undo" 
          class="text-4xl"
        />
        <font-awesome-icon
          icon="play" 
          class="text-4xl text-teal-500"
        />
        <font-awesome-icon
          icon="redo" 
          class="text-4xl"
        />
        <font-awesome-icon
          icon="step-forward" 
          class="text-4xl"
        />
      </div>
      <div>
        {{ humanFriendlyPlayhead }} / {{ humanFriendlyDuration }}
      </div>
      <div>
        <div class="flex items-center justify-between">
          <div class="relative w-full">
            <input 
              id="play-slider"
              class="w-full appearance-none bg-gray-200"
              type="range"
              min="0"
              :max="playingEpisode.duration"
              :value="playingEpisode.playhead"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Helpers from '../Helpers'

export default {
  computed: {
    playingEpisode() {
      return this.$store.state.playingEpisode
    },

    humanFriendlyDuration() {
      return Helpers.floatToISO(this.playingEpisode.duration)
    },

    humanFriendlyPlayhead() {
      return Helpers.floatToISO(this.playingEpisode.playhead)
    }
  },

  created() {
    Helpers.dexieDB.episodes
      .filter(ep => ep.currently_playing == true)
      .toArray().then(result => {
        this.$store.dispatch('playEpisode', result[0]._id)
      })
  }
}
</script>