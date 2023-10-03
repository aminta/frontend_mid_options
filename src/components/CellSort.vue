<script>
import { useProjectsStore } from "@/stores/projects.js";
import { mapStores } from "pinia";
import Cell from "./fragments/Cell.vue";

export default {
  extends: Cell,
  props: {
    field: String,
  },
  data() {
    const store = useProjectsStore();
    return {
      isAsc: store.sortOrder === "asc" && store.sortBy === this.field,
    };
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
    row() {
      return {
        "flex flex-row gap-4": true,
      };
    },
  },
  watch: {
    isAsc(newValue, oldValue) {
      let sortOrder = newValue ? "asc" : "desc";
      this.projectsStore.setSortOrder(sortOrder);
      this.projectsStore.setSortBy(this.field);
    },
  },
  methods: {
    toggleSortOrder() {
      this.isAsc = !this.isAsc;
    },
  },
};
</script>

<template>
  <section :class="classes">
    <main :class="row">
      <slot></slot>
      <i>
        <font-awesome-icon @click="toggleSortOrder" :icon="iconClasses" />
      </i>
    </main>
  </section>
</template>

<style lang="scss" scoped></style>
