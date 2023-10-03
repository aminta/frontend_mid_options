<script>
import Cell from "./fragments/Cell.vue";
import Date from "./fragments/cells/Date.vue";
import StarToggler from "./fragments/cells/StarToggler.vue";
import Layout from "./fragments/layout/Layout.vue";

export default {
  components: {
    Cell,
    Layout,
  },
  provide() {
    return {
      id: this.id,
    };
  },
  props: {
    name: String,
    description: String,
    createdAt: String,
    stars: Number,
  },

  data() {
    return {
      Date: Date,
      StarToggler: StarToggler,
      id: this.$.vnode.key,
    };
  },
};
</script>

<template>
  <Layout :columns="4">
    <Cell>
      <h3 class="text-lg font-bold">{{ name }}</h3>
      <p class="text-zinc-400">{{ description }}</p>
    </Cell>

    <Cell centered>
      {{ stars }}
    </Cell>

    <Cell right :renderer="Date">
      {{ createdAt }}
    </Cell>

    <Cell :renderer="StarToggler" />
  </Layout>
</template>
