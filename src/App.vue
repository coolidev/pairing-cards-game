<template>
  <v-app dark>
    <v-container>
      <v-layout class="font-weight-bold display-3 purple--text" justify-center mb-4>Match Pairs</v-layout>
      <v-layout mb-4>
        <timer :progress="progress" :time="time"></timer>
        <status-bar :turns="turns" :matches="matches"></status-bar>
      </v-layout>
      <cards-board :cards="cards" @flipcard="flipCard"></cards-board>
    </v-container>
    <splash :showSplash="showSplash" :result="result" :score="score" @resetgame="resetGame"></splash>
  </v-app>
</template>

<script>
const cardsSet = [
  {
    name: "php",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/php-logo_1.png"
  },
  {
    name: "css3",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/css3-logo.png"
  },
  {
    name: "html5",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/html5-logo.png"
  },
  {
    name: "jquery",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/jquery-logo.png"
  },
  {
    name: "javascript",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/js-logo.png"
  },
  {
    name: "node",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/nodejs-logo.png"
  },
  {
    name: "photoshop",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/photoshop-logo.png"
  },
  {
    name: "python",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/python-logo.png"
  },
  {
    name: "rails",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/rails-logo.png"
  },
  {
    name: "sass",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/sass-logo.png"
  },
  {
    name: "sublime",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/sublime-logo.png"
  },
  {
    name: "wordpress",
    img: "https://s3-us-west-2.amazonaws.com/s.cdpn.io/74196/wordpress-logo.png"
  }
];

let shuffleCards = () => {
  let cards = [].concat(_.cloneDeep(cardsSet), _.cloneDeep(cardsSet));
  return _.shuffle(cards);
};

import CardsBoard from "./components/CardsBoard";
import StatusBar from "./components/StatusBar";
import Timer from "./components/Timer";
import Splash from "./components/Splash";
import _ from "lodash";
import moment from "moment";

export default {
  name: "App",
  components: {
    CardsBoard,
    StatusBar,
    Timer,
    Splash
  },
  data() {
    return {
      cards: [],
      started: false,
      duration: 120000,
      progress: 0,
      time: null,
      timer: null,
      flipBackTimer: null,
      turns: 0,
      matches: 0,
      showSplash: false,
      result: "",
      score: 0
    };
  },
  created() {
    this.resetGame();
  },
  methods: {
    resetGame() {
      this.started = false;
      this.progress = 0;
      this.time = null;
      this.turns = 0;
      this.matches = 0;
      this.showSplash = false;
      this.score = 0;

      let cards = shuffleCards();

      _.each(cards, card => {
        card.flipped = false;
        card.found = false;
      });

      this.cards = cards;
    },

    flippedCards() {
      return _.filter(this.cards, card => card.flipped);
    },

    sameFlippedCard() {
      let flippedCards = this.flippedCards();
      if (flippedCards.length == 2) {
        if (flippedCards[0].name == flippedCards[1].name) return true;
      }
    },

    setCardFounds() {
      _.each(this.cards, card => {
        if (card.flipped) card.found = true;
      });
    },

    checkAllFound() {
      let foundCards = _.filter(this.cards, card => card.found);
      if (foundCards.length == this.cards.length) return true;
    },

    clearFlips() {
      _.map(this.cards, card => (card.flipped = false));
    },

    clearFlipBackTimer() {
      clearTimeout(this.flipBackTimer);
      this.flipBackTimer = null;
    },

    startGame() {
      this.started = true;
      let duration = this.duration;
      this.timer = setInterval(() => {
        if (duration == 1000) {
          this.finishGame();
        }
        duration -= 1000;
        this.time = moment(duration).format("mm:ss");
        this.progress = (duration / this.duration) * 100;
      }, 1000);
    },

    finishGame() {
      this.started = false;
      clearInterval(this.timer);
      this.score = Math.round(this.progress) * 10 * this.matches;

      if (this.matches == cardsSet.length) {
        this.result = "Win";
      } else {
        this.result = "Lose";
      }

      this.showSplash = true;
    },

    flipCard(card) {
      if (card.found || card.flipped) return;

      if (!this.started) {
        this.startGame();
      }

      let flipCount = this.flippedCards().length;
      if (flipCount == 0) {
        card.flipped = !card.flipped;
      } else if (flipCount == 1) {
        card.flipped = !card.flipped;
        this.turns += 1;

        if (this.sameFlippedCard()) {
          this.matches += 1;
          this.flipBackTimer = setTimeout(() => {
            this.clearFlipBackTimer();
            this.setCardFounds();
            this.clearFlips();

            if (this.checkAllFound()) {
              this.finishGame();
            }
          }, 1000);
        } else {
          this.flipBackTimer = setTimeout(() => {
            this.clearFlipBackTimer();
            this.clearFlips();
          }, 1000);
        }
      }
    }
  }
};
</script>
