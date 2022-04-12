<template>
  <fragment v-if="!isMenuItem">
    <p v-if="properties" :class="['notion-text', blockColorClass()]">
      <NotionTextRenderer :text="title" v-bind="pass" />
    </p>
    <div v-else class="notion-blank">&nbsp;</div>
  </fragment>
  <span v-else :id="pass.contentId" />
</template>

<script>
import { Blockable } from "@/lib/blockable";
import NotionTextRenderer from "@/blocks/helpers/text-renderer";
import Fragment from "@/blocks/helpers/fragment";

export default {
  extends: Blockable,
  name: "NotionText",
  components: { NotionTextRenderer, Fragment },
  beforeMount() {
    if (this.isMenuItem) {
      this.$parent.$emit("side-menu", this.title[0][0], this.pass.contentId);
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
