<template>
  <span v-if="isMenuItem" :id="pass.contentId" />
  <span v-else-if="isMasiveStarter" class="notion-masive" />
  <fragment v-else>
    <p v-if="properties" :class="['notion-text', blockColorClass()]">
      <NotionTextRenderer :text="title" v-bind="pass" />
    </p>
    <div v-else class="notion-blank">&nbsp;</div>
  </fragment>
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
      this.$root.$emit("side-menu", this.title[0][0], this.pass.contentId);
    }
  },
  computed: {
    isMenuItem() {
      const { title } = this;
      return title && title[0] && title[0][0] && title[0][0].includes("$");
    },
    isMasiveStarter() {
      const { title } = this;
      return title && title[0] && title[0][0] && title[0][0].includes("##");
    }
  }
};
</script>
