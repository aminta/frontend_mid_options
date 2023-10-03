<script>
import { formatDate } from "@/utilities/formatters.js";
export default {
  props: {
    content: String,
  },
  computed: {
    date() {
      return formatDate(this.content);
    },
  },
};
</script>

<template>
  {{ date }}
</template>

<style lang="scss" scoped></style>
