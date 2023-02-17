<script>
import { useProjectsStore } from "@/stores/projects.js";
import { mapStores } from "pinia";

export default {
  data() {
    const store = useProjectsStore();
    return {
      isPressed: store.showOnlyTopThree,
    };
  },

  methods: {
    toggleButton() {
      this.isPressed = !this.isPressed;
    },
  },

  computed: {
    buttonClasses() {
      return {
        "border border border-gray-300 rounded-md pl-8 p-1.5 hover:bg-gray-300": true,
        "bg-gray-300": this.isPressed,
      };
    },
    iconClasses() {
      return [
        "fa-solid",
        !this.isPressed ? "fa-ranking-star" : "fa-circle-xmark",
      ];
    },
    ...mapStores(useProjectsStore),
  },

  watch: {
    isPressed(newValue, oldValue) {
      this.projectsStore.setShowOnlyTopThreProjects(newValue);
    },
  },
};
</script>

<template>
  <div class="relative cursor-pointer" @click="toggleButton">
    <font-awesome-icon
      :icon="iconClasses"
      class="absolute absolute left-[10px] z-10 top-[10px]"
    />
    <button :class="buttonClasses">
      {{ isPressed ? "Show all" : "Show Only Top 3 Starred" }}
    </button>
  </div>
</template>

<style lang="scss" scoped></style>
