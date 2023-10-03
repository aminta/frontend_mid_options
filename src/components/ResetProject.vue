<script>
import Modal from "@/components/Modal.vue";
import useStorage from "@/composables/useStorage.js";

export default {
  components: {
    Modal,
  },

  // setup() itself does not have access to the component instance - this will have a value of undefined inside setup(). You can access Composition-API-exposed values from Options API, but not the other way around.
  setup() {
    const { reset } = useStorage("projects");
    return { reset };
  },

  data() {
    return {
      showModal: false,
    };
  },
  methods: {
    onDismiss() {
      this.showModal = false;
    },
    onConfirm() {
      this.showModal = false;
      this.resetApp();
    },
    resetApp() {
      this.reset();
      location.reload();
    },
  },
};
</script>

<template>
  <button
    @click="showModal = true"
    class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded disabled:bg-gray-400 mt-10"
  >
    Reset App
  </button>
  <Teleport to="body">
    <Modal
      :show="showModal"
      @dismiss="onDismiss"
      @confirm="onConfirm"
      dismissText="Cancel"
      saveText="Confirm"
    >
      <template #body>
        <p>
          Are you sure you want to reset the project to its initial state and
          reload the page?
        </p>
      </template>
    </Modal>
  </Teleport>
</template>
