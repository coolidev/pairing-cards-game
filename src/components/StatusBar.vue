<template>
  <v-layout align-center class="headline">
    <span class="mx-3">Turns: {{ turns }}</span>
    <v-divider vertical></v-divider>
    <span class="mx-3">Matches: {{ matches }}</span>
  </v-layout>
</template>

<script>
export default {
  props: ["turns", "matches"]
};
</script>
