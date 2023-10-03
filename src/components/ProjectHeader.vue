<script setup>
import CellSort from "./CellSort.vue";
import Layout from "./fragments/layout/Layout.vue";
</script>

<template>
  <Layout :columns="4">
    <CellSort field="name">
      <h3 class="text-lg font-bold">Nome e descrizione</h3>
    </CellSort>

    <CellSort centered field="stars"> Rating </CellSort>

    <CellSort right field="createdAt"> Data di creazione </CellSort>
  </Layout>
</template>

<style lang="scss" scoped></style>
