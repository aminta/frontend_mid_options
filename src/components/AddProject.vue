<script>
import Modal from "@/components/Modal.vue";
import { useProjectsStore } from "@/stores/projects.js";
import { mapStores } from "pinia";

export default {
  components: {
    Modal,
  },
  data() {
    return {
      showModal: false,
      newProject: {
        name: "",
        description: "",
      },
      validationErrors: [],
    };
  },
  mounted() {},
  created() {},
  methods: {
    onClose() {
      this.showModal = false;
      this.newProject.name = "";
      this.newProject.description = "";
    },
    onSave() {
      const noNameInserted =
        this.newProject.name.trim() === ""
          ? "Attenzione, inserire descrizione progetto"
          : false;
      const noDescriptionInserted =
        this.newProject.description.trim() === ""
          ? "Attenzione, inserire nome progetto"
          : false;

      if (noNameInserted || noDescriptionInserted) {
        if (noNameInserted && !this.validationErrors.includes(noNameInserted))
          this.validationErrors.push(noNameInserted);
        if (
          noDescriptionInserted &&
          !this.validationErrors.includes(noDescriptionInserted)
        )
          this.validationErrors.push(noDescriptionInserted);
      } else {
        this.projectsStore.addNewProject(this.newProject);
        this.showModal = false;
      }
      this.newProject.name = "";
      this.newProject.description = "";
    },
  },
  computed: {
    ...mapStores(useProjectsStore),
  },
  props: {},
};
</script>
<template>
  <button
    class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded disabled:bg-gray-400"
    @click="showModal = true"
  >
    Add new project
  </button>
  <Teleport to="body">
    <Modal :show="showModal" @dismiss="onClose" @confirm="onSave">
      <template #body>
        <form class="mt-6">
          <div class="flex flex-col gap-4">
            <div class="flex gap-2">
              <label class="min-w-[30%]">Name</label>
              <input
                class="flex-1 border-b border-gray-300 p-2 pl-0 pt-0 focus:outline-none focus:border-sky-500"
                type="text"
                placeholder="Project name..."
                v-model="newProject.name"
              />
            </div>
            <div class="flex gap-2">
              <label class="min-w-[30%]">Description</label>
              <textarea
                class="flex-1 border-b border-gray-300 focus:outline-none focus:border-sky-500"
                type="text"
                placeholder="Project description..."
                v-model="newProject.description"
              />
            </div>
          </div>
        </form>
        {{ validationError ? validationError : "" }}
      </template>
    </Modal>
  </Teleport>
</template>
