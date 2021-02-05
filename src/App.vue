<template>
  <poker-game @finished-game="checkCards" class="container"></poker-game>
  <hr />
  <poker-game @finished-game="checkCards" class="container"></poker-game>
  <hr />

  <div class="container">
    <h2 style="color:green" v-if="isGameFinished">
      Game finished. Winning hand {{ winner }} 
    </h2>
  </div>
</template>

<script>
import PokerGame from "./components/PokerGame.vue";
import { Hand } from "pokersolver";

export default {
  data() {
    return {
      hands: [],
      winner: '',
    };
  },
  created() {},
  components: {
    PokerGame,
  },
  computed: {
    isGameFinished() {
      return this.hands.length == 2;
    },
    whoWinsColor() {
      let colorPerWin = ["grey", "blue", "red"];
      return colorPerWin[this.winner];
    },
  },
  methods: {
    checkCards(selectedCards) {
      //this.hands.push(selectedCards);

      let hand = selectedCards.map((card) => {
        card = card.replace("spades", "s");
        card = card.replace("clubs", "c");
        card = card.replace("hearts", "h");
        card = card.replace("diamonds", "d");
        card = card.replace("10", "T");
        card = card.replace("ace", "A");
        card = card.replace("jack", "J");
        card = card.replace("queen", "Q");
        card = card.replace("king", "K");

        return card;
      });
      this.hands.push(hand);

      if (this.hands.length == 2) {
        let hand1 = Hand.solve(this.hands[0]);
        let hand2 = Hand.solve(this.hands[1]);
        var winner = Hand.winners([hand1, hand2]); // hand2
        console.log(winner)
        this.winner = winner[0].cards.map(card => {
          return  card.value + card.suit
        })
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
@media (min-width: 768px) {
  .container {
    width: 750px;
  }
}
@media (min-width: 992px) {
  .container {
    width: 970px;
  }
}
@media (min-width: 1200px) {
  .container {
    width: 1170px;
  }
}
</style>