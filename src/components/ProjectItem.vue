<script>
import ColumnSort from "./ColumnSort.vue";
import { formatDate } from "@/utilities/formatters.js";
import { useProjectsStore } from "@/stores/projects.js";
import { mapStores } from "pinia";

export default {
  components: {
    ColumnSort,
  },

  data() {
    const store = useProjectsStore();
    const item = store.projects?.find(
      (project) => project.id === this.$.vnode.key
    );
    return {
      starred: item?.starredFromCurrentUser,
    };
  },

  watch: {
    // whenever starred changes, this function will run
    starred(newStarred, oldStarred) {
      this.projectsStore.toggleStarToAProject(this.$.vnode.key, newStarred);
      this.projectsStore.toggleStarredFromCurrentUser(
        this.$.vnode.key,
        newStarred
      );
    },
  },

  methods: {
    toggleStar() {
      return (this.starred = !this.starred);
    },
  },

  computed: {
    iconClasses() {
      return this.starred ? "fa-solid fa-star" : "fa-regular fa-star";
    },
    buttonClasses() {
      return {
        "flex gap-2 border  rounded-[18px] p-3 hover:bg-gray-200 hover:border-gray-400": true,
        "bg-gray-200 border-gray-400": this.starred,
        "bg-transparent border-gray-300": !this.starred,
      };
    },
    ...mapStores(useProjectsStore),
    classObject() {
      return {
        flex: this.isHeader,
        "flex-row": this.isHeader,
        "justify-between": this.isHeader,
      };
    },
    date() {
      return formatDate(this.createdAt);
    },
  },

  props: {
    name: String,
    description: String,
    "created-at": String,
    stars: Number,
    isHeader: Boolean,
  },
};
</script>

<template>
  <li class="grid grid-cols-4 gap-y-10 border-b border-gray-300 pb-2">
    <header :class="classObject">
      <h3 class="text-lg font-bold">{{ name }}</h3>
      <p v-if="!isHeader" class="text-zinc-400">{{ description }}</p>
      <ColumnSort v-if="isHeader" field="name" />
    </header>
    <main class="text-center">
      {{ stars }}
      <ColumnSort v-if="isHeader" field="stars" />
    </main>
    <footer class="text-right">
      {{ date }}
      <ColumnSort v-if="isHeader" field="createdAt" />
    </footer>
    <aside v-if="!isHeader" class="flex justify-end items-baseline">
      <button :class="buttonClasses" @click="toggleStar">
        <span class="leading-none">Rate</span>
        <font-awesome-icon :icon="iconClasses" />
      </button>
    </aside>
  </li>
</template>

<style lang="scss" scoped></style>
