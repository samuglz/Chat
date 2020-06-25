<template>
  <div class="ml-2 bg-white w-56 h-56 border border-gray-300 py-4 px-1 rounded overflow-y-auto">
    <div class="md:px-0 px-2">
      <search-bar class="w-full"/>
    </div>
    <div class="grid grid-cols-7">
      <div
        class="flex justify-center items-center cursor-pointer"
        @click="handleClick(emoji.emoji)"
        v-for="(emoji, index) in emojis"
        :key="`${index}-${emoji.description}`"
      >{{emoji.emoji}}</div>
    </div>
  </div>
</template>
<script>
import emojisJSON from "../assets/emojis.json";
import SearchBar from "@/components/SearchBar";
import { EventBus } from "../events";
export default {
  name: "emoji-panel",
  mounted() {
    for (let category in this.emojisJSON) {
      this.emojisJSON[category].forEach(emojis => {
        this.totalEmojis.push(emojis);
      });
    }
    this.emojis = this.totalEmojis;

    EventBus.$on("filterEmoji", filter => {
      this.emojis = this.totalEmojis.filter(emoji => {
        return emoji.description.toLowerCase().includes(filter.toLowerCase());
      });
      console.log(this.emojis);
    });
  },
  data() {
    return {
      emojisJSON,
      emojis: [],
      totalEmojis: []
    };
  },
  components: {
    SearchBar
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

