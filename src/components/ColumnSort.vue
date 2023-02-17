<script>
import { useProjectsStore } from "@/stores/projects.js";
import { mapStores } from "pinia";

export default {
  data() {
    const store = useProjectsStore();
    return {
      isAsc: store.sortOrder === "asc" && store.sortBy === this.field,
    };
  },

  methods: {
    toggleSortOrder() {
      this.isAsc = !this.isAsc;
    },
  },

  computed: {
    ...mapStores(useProjectsStore),
    iconClasses() {
      return [
        "fa-solid",
        this.isAsc ? "fa-sort-up" : "",
        !this.isAsc ? "fa-sort-down" : "",
      ].join(" ");
    },
  },

  props: {
    field: String,
  },

  watch: {
    isAsc(newValue, oldValue) {
      let sortOrder = newValue ? "asc" : "desc";
      this.projectsStore.setSortOrder(sortOrder);
      this.projectsStore.setSortBy(this.field);
    },
  },
};
</script>

<template>
  <i>
    <font-awesome-icon @click="toggleSortOrder" :icon="iconClasses" />
  </i>
</template>

<style lang="scss" scoped></style>
