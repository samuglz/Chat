<template>
  <div
    ref="chatPanel"
    class="w-3/4 h-64 rounded border bg-gray-300 border-gray-500 mx-2 flex overflow-y-auto flex-col items-end"
  >
    <div
      class="bg-green-100 my-2 mr-2 rounded text-center px-2"
      v-for="(message, index) in messages"
      :key="index"
    >{{message}}</div>
  </div>
</template>
<script>
import { EventBus } from "../events";
export default {
  name: "Chat-Panel",
  mounted() {
    EventBus.$on("sendMessage", text => {
      this.messages.push(`${text}`);
      if (this.$refs.chatPanel)
        this.$refs.chatPanel.scrollTo(0, this.$refs.chatPanel.scrollHeight);
      EventBus.$emit("clearInput");
    });
  },
  data() {
    return {
      messages: []
    };
  }
};
</script>
<style>
</style>


