<template>
  <input
    class="mb-2 border rounded py-1 px-2 focus:outline-none shadow"
    type="text"
    placeholder="Busca tu emoji..."
    v-model="filter"
  >
</template>
<script>
import { EventBus } from "../events";
export default {
  name: "searchBar",
  data() {
    return {
      filter: ""
    };
  },
  watch: {
    filter() {
      EventBus.$emit("filterEmoji", this.filter);
    }
  }
};
</script>

