<template>
  <v-layout>
    <v-flex xs12 sm10 offset-sm1 lg8 offset-lg2>
      <v-card>
        <div class="flex-container">
          <div
            v-for="(card, i) in cards"
            :key="i"
            class="card"
            :class="{ flipped: card.flipped, found: card.found }"
            @click="flipCard(card)"
          >
            <div class="back"></div>
            <div class="front" :style="{ backgroundImage: 'url(' + card.img + ')' }"></div>
          </div>
        </div>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
export default {
  props: ["cards"],
  methods: {
    flipCard(card) {
      this.$emit("flipcard", card);
    }
  }
};
</script>

<style>
.flex-container {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
}
.card {
  position: relative;
  width: 100px;
  height: 150px;
  margin: 1px;
  transition: opacity 0.5s;
}
@media only screen and (max-width: 768px) {
  .card {
    width: 50px;
    height: 75px;
  }
}
.card .front,
.card .back {
  border-radius: 5px;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  transition: transform 0.3s;
  transform-style: preserve-3d;
}
.card .back {
  background-image: url("https://s3-us-west-2.amazonaws.com/s.cdpn.io/102308/card_backside.jpg");
  background-size: 100%;
  background-repeat: no-repeat;
}
.card .front {
  transform: rotateY(-180deg);
  background-size: 100%;
  background-repeat: no-repeat;
  background-position: center;
  background-color: white;
}
.card.flipped .back,
.card.found .back {
  transform: rotateY(180deg);
}
.card.flipped .front,
.card.found .front {
  transform: rotateY(0deg);
}
.card.found {
  opacity: 0.3;
}
</style>
