<template>
  <fragment>
    <div class="m-2 flex w-3/4">
      <button
        @click="openEmojis()"
        ref="dropdown"
        class="dropdown focus:outline-none shadow border-gray-300 border px-1 rounded-tl rounded-bl py-1"
      >
        <span>{{emoji(emojiButtonCode)}}</span>
        <div ref="content" class="dropdown-content">
          <emoji-panel/>
        </div>
      </button>
      <input
        ref="inputText"
        type="text"
        class="focus:outline-none shadow flex-grow w-3/4 px-4 border-t border-b border-r text-gray-700 rounded-br rounded-tr"
        placeholder="Escriba su mensaje..."
        v-model="text"
        @keyup.enter="sendMessage()"
      >
    </div>
  </fragment>
</template>
<script>
import { Fragment } from "vue-fragment";
import EmojiPanel from "@/components/EmojiPanel";
import { EventBus } from "../events";
export default {
  name: "emoji-input",
  data() {
    return {
      emojiButtonCode: "0x1F600",
      text: ""
    };
  },
  mounted() {
    if (this.$refs.inputText) this.$refs.inputText.focus();
    EventBus.$on("addEmoji", emoji => {
      this.text = `${this.text}${emoji}`;
      if (this.$refs.inputText) this.$refs.inputText.focus();
    });
    EventBus.$on("clearInput", () => {
      this.text = "";
    });
  },
  methods: {
    emoji(emojiCode) {
      return String.fromCodePoint(emojiCode);
    },
    openEmojis() {
      if (this.$refs.dropdown)
        this.$refs.dropdown.classList.toggle("display-block");
      if (this.$refs.content)
        this.$refs.content.classList.toggle("display-block");
    },
    sendMessage() {
      EventBus.$emit("sendMessage", this.text);
    }
  },
  components: {
    Fragment,
    EmojiPanel
  }
};
</script>
<style scoped>
.dropdown {
  position: relative;
  display: inline-block;
}
.dropdown-content {
  display: none;
  position: absolute;
  z-index: 1;
  overflow-y: auto;
  top: 3em;
  left: 0;
}
.display-block {
  display: block;
}
</style>


