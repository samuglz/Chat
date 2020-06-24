<template>
  <div
    class="flex flex-wrap w-56 h-56 justify-around border border-gray-300 py-4 px-1 rounded overflow-y-auto"
  >
    <div
      @click="handleClick(emoji.emoji)"
      v-for="(emoji, index) in emojis"
      :key="`${index}-${emoji.description}`"
    >{{emoji.emoji}}</div>
  </div>
</template>
<script>
import emojisJSON from "../assets/emojis.json";
import { EventBus } from "../events";
export default {
  name: "emoji-panel",
  mounted() {
    for (let category in this.emojisJSON) {
      this.emojisJSON[category].forEach(emojis => {
        this.emojis.push(emojis);
      });
    }
  },
  data() {
    return {
      emojisJSON,
      emojis: []
    };
  },
  methods: {
    handleClick(emoji) {
      EventBus.$emit("addEmoji", emoji);
    }
  }
};
</script>
<style>
</style>

