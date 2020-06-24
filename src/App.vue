<template>
  <div class="flex flex-col items-center justify-center">
    <chat-panel class="mt-2"/>
    <emoji-input/>
  </div>
</template>

<script>
import { Fragment } from "vue-fragment";
import EmojiInput from "@/components/EmojiInput";
import ChatPanel from "@/components/ChatPanel";
export default {
  name: "App",
  components: {
    Fragment,
    EmojiInput,
    ChatPanel
  }
};
</script>

<style>
</style>
