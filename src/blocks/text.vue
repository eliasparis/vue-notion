<template>
  <fragment v-if="!isMenuItem">
    <p v-if="properties" :class="['notion-text', blockColorClass()]">
      <NotionTextRenderer :text="title" v-bind="pass" />
    </p>
    <div v-else class="notion-blank">&nbsp;</div>
  </fragment>
</template>

<script>
import { Blockable } from "@/lib/blockable";
import NotionTextRenderer from "@/blocks/helpers/text-renderer";
import fragment from "@/blocks/helpers/fragment";

export default {
  extends: Blockable,
  name: "NotionText",
  components: { NotionTextRenderer, fragment },
  beforeMount() {
    if (this.isMenuItem) {
      this.$root.$emit("side-menu", this.title[0][0]);
    }
  },
  computed: {
    isMenuItem() {
      const { title } = this;
      return title && title[0] && title[0][0] && title[0][0].includes("$");
    }
  }
};
</script>
