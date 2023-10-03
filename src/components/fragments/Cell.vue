<script>
export default {
  inject: ["id"],
  props: {
    renderer: Function,
    centered: Boolean,
    right: Boolean,
    left: Boolean,
  },
  data() {
    return {
      textContent:
        typeof this.$slots.default === "function"
          ? this.$slots.default()[0].children
          : "",
    };
  },

  computed: {
    classes() {
      return {
        "flex flex-col": true,
        "items-center": this.centered,
        "items-end": this.right,
        "items-start": this.left,
      };
    },
  },
};
</script>

<template>
  <section :class="classes">
    <KeepAlive>
      <component :is="renderer" :content="textContent" :id="id" />
    </KeepAlive>
    <slot v-if="!renderer"></slot>
  </section>
</template>

<style lang="scss" scoped></style>
