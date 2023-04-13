<template>
  <v-layout justify-end>
    <v-progress-circular
      :rotate="-90"
      :value="progress"
      :size="70"
      :width="9"
      :color="color"
    >{{time}}</v-progress-circular>
  </v-layout>
</template>

<script>
export default {
  props: ["progress", "time"],
  data() {
    return {
      color: "success"
    };
  },
  watch: {
    progress() {
      if (this.progress <= 10) {
        this.color = "red";
      } else {
        this.color = "success";
      }
    }
  }
};
</script>
