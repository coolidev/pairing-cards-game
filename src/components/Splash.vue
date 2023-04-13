<template>
  <v-layout row>
    <v-dialog v-model="showSplash" persistent max-width="300">
      <v-card class="text-xs-center">
        <v-card-text>
          <div class="headline mb-4">You {{result}}!</div>
          <div>Score: {{score}}</div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" flat @click="resetGame">Start New Game</v-btn>
          <v-spacer></v-spacer>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
export default {
  props: ["showSplash", "result", "score"],
  methods: {
    resetGame() {
      this.$emit("resetgame");
    }
  }
};
</script>
