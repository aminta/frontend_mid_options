<script>
import Modal from "@/components/Modal.vue";
import { useProjectsStore } from "@/stores/projects.js";
import { mapStores } from "pinia";
import Errors from "@/components/Errors.vue";

export default {
  components: {
    Modal,
    Errors,
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

  created() {
    const errorsMessages = {
      name: "Attenzione, inserire nome progetto",
      description: "Attenzione, inserire descrizione progetto",
    };

    this.errorsMessages = errorsMessages;
  },

  methods: {
    onClose() {
      this.showModal = false;
      this.newProject.name = "";
      this.newProject.description = "";
    },
    onSave() {
      this.validationErrors = [];

      const thereAreErrros = this.checkEmptyFields(this.fieldsToCheck);

      if (!thereAreErrros) {
        this.validationErrors = [];
        this.projectsStore.addNewProject(this.newProject);
        this.showModal = false;
        this.newProject.name = "";
        this.newProject.description = "";
      }
    },
    checkEmptyFields(fields) {
      this.validationErrors = [];
      let result = false;
      for (const property in fields) {
        const value = fields[property];
        if (this.isEmpty(value)) {
          result = true;
          this.validationErrors.push(this.errorsMessages[property]);
        }
      }
      return result;
    },
    isEmpty(el) {
      return el.trim() === "";
    },
  },

  computed: {
    ...mapStores(useProjectsStore),

    fieldsToCheck() {
      return {
        name: this.newProject.name,
        description: this.newProject.description,
      };
    },
  },
  watch: {
    "newProject.name": function (newVal, oldVal) {
      this.checkEmptyFields(this.fieldsToCheck);
    },
    "newProject.description": function (newVal, oldVal) {
      this.checkEmptyFields(this.fieldsToCheck);
    },
  },
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
        <Errors :errors="validationErrors" />
      </template>
    </Modal>
  </Teleport>
</template>
