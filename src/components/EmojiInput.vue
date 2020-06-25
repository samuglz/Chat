<template>
  <fragment>
    <div class="m-2 flex w-3/4">
      <button
        @click="handleMenu()"
        ref="dropdown"
        class="dropdown focus:outline-none shadow border-gray-300 border px-1 rounded-tl rounded-bl py-1 hover:bg-gray-300"
      >
        <span>{{emoji(emojiButtonCode)}}</span>
      </button>
      <input
        ref="inputText"
        type="text"
        class="focus:outline-none shadow flex-grow w-3/4 px-4 border-t border-b text-gray-700"
        placeholder="Escriba su mensaje..."
        v-model="text"
        @keyup.enter="sendMessage()"
      >
      <button
        @click="sendMessage()"
        class="focus:outline-none bg-green-700 shadow text-white hover:bg-green-500 py-2 px-2 border-t border-b border-r rounded-br rounded-tr border-gray-300 border"
      >Send</button>
    </div>
    <div ref="content" class="dropdown-content">
      <emoji-panel/>
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
      text: "",
      isOpen: false
    };
  },
  mounted() {
    // if (this.$refs.inputText) this.$refs.inputText.focus();
    EventBus.$on("addEmoji", emoji => {
      this.text = `${this.text}${emoji}`;
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
        this.$refs.dropdown.classList.add("display-block");
      if (this.$refs.content) this.$refs.content.classList.add("display-block");
      this.isOpen = !this.isOpen;
    },
    sendMessage() {
      this.isOpen = true;
      this.handleMenu();
      EventBus.$emit("sendMessage", this.text);
    },
    closeEmojis() {
      if (this.$refs.dropdown)
        this.$refs.dropdown.classList.remove("display-block");
      if (this.$refs.content)
        this.$refs.content.classList.remove("display-block");
      this.isOpen = !this.isOpen;
    },
    handleMenu() {
      this.isOpen ? this.closeEmojis() : this.openEmojis();
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
  top: 19.5em;
}
.display-block {
  display: block;
}
</style>


