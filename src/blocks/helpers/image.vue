<template>
  <div v-if="f.block_aspect_ratio" :style="style">
    <component
      v-if="hasImageComponent"
      :is="imageOptions.component"
      class="notion-image-inset"
      :alt="alt || 'Notion image'"
      v-bind="imageProps"
    />
    <img
      v-else
      class="notion-image-inset"
      :alt="alt || 'Notion image'"
      v-bind="imageProps"
    />
    <!-- {{ f }} -->
  </div>
  <component
    v-else-if="hasImageComponent"
    :is="imageOptions.component"
    :alt="alt || 'Notion image'"
    v-bind="imageProps"
  />
  <img v-else :alt="alt" v-bind="imageProps" />
</template>

<script>
import { Blockable, blockComputed } from "@/lib/blockable";

export default {
  extends: Blockable,
  name: "NotionImage",
  data() {
    return {
      windowWidth: window.innerWidth
    };
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener("resize", this.onResize);
    });
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.onResize);
  },
  methods: {
    onResize() {
      this.windowWidth = window.innerWidth;
    }
  },
  computed: {
    ...blockComputed,
    hasImageComponent() {
      return !!this.imageOptions?.component;
    },
    imageProps() {
      const { component, ...attrs } = this.imageOptions || {};
      return {
        ...attrs,
        [this.imageOptions?.src || "src"]: this.src
      };
    },
    style() {
      const margin = (this.f.block_width - 708) / 2;

      const base = {
        width: `${this.f.block_width}px`,
        "max-width": "100%",
        position: "relative",
        margin: "0 auto"
      };

      if (this.f.block_page_width) {
        base.width = "100%";
        base["margin-left"] = "auto";
      } else {
        base["max-width"] = "98vw";
        base["margin-left"] = `-${margin}px`;
        if (this.windowWidth < this.f.block_width) {
          base["margin-left"] = `calc((98vw - 708px)/2 * -1)`;
          base.width = "98vw";
        }
      }
      return base;
    }
  }
};
</script>
