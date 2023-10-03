<script>
import { useProjectsStore } from "@/stores/projects.js";
import { mapStores } from "pinia";
export default {
  props: {
    content: String,
    id: Number,
  },
  data() {
    const store = useProjectsStore();
    const item = store.projects?.find((project) => project.id === this.id);
    return {
      starred: item?.starredFromCurrentUser,
    };
  },
  computed: {
    ...mapStores(useProjectsStore),
    buttonClasses() {
      return {
        "flex gap-2 border  rounded-[18px] p-3 hover:bg-gray-200 hover:border-gray-400": true,
        "bg-gray-200 border-gray-400": this.starred,
        "bg-transparent border-gray-300": !this.starred,
      };
    },
    iconClasses() {
      return this.starred ? "fa-solid fa-star" : "fa-regular fa-star";
    },
  },
  watch: {
    // whenever starred changes, this function will run
    starred(newStarred, oldStarred) {
      this.projectsStore.toggleStarToAProject(this.id, newStarred);
      this.projectsStore.toggleStarredFromCurrentUser(this.id, newStarred);
    },
  },

  methods: {
    toggleStar() {
      return (this.starred = !this.starred);
    },
  },
};
</script>

<template>
  <aside v-if="!isHeader" class="flex justify-end items-baseline">
    <button :class="buttonClasses" @click="toggleStar">
      <span class="leading-none">Rate</span>
      <font-awesome-icon :icon="iconClasses" />
    </button>
  </aside>
</template>

<style lang="scss" scoped></style>
