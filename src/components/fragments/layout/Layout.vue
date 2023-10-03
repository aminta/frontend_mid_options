<script>
export default {
  computed: {
    classes() {
      return {
        [`grid grid-cols-${this.columns} gap-y-10 border-b border-gray-300 pb-2`]: true,
      };
    },
  },
  props: {
    columns: Number,
  },
};
</script>

<template>
  <li :class="classes">
    <slot></slot>
  </li>
</template>
