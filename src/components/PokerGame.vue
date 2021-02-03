<template>
    <div v-if="notYetFiveCardsFlippedUp" >
  <poker-card @turnedCard="checkTrunedCards"
    v-for="card in deck"
    :card-image="card.url"
    :card-name="card.name"
    :key="card.name"
  ></poker-card>
  </div>
  <h1 v-else> 5 cards turned up!</h1>
</template>

<script>
import PokerCard from "./PokerCard.vue";
export default {
  components: {
    PokerCard,
  },
  data() {
    return {
      flippedUpCards: 0,
      suits: ["spades", "clubs", "hearts", "diamonds"],
      numbers: [
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "10",
        "jack",
        "queen",
        "king",
        "ace",
      ],
      deck: [],
    };
  },
  beforeMount() {
    this.generateDeck();
    this.shuffleDeck(this.deck);
  },
  computed: {
      notYetFiveCardsFlippedUp() {
          return this.flippedUpCards != 5
      }
  },
  methods: {
    generateDeck() {
      const deck = [];
      for (const suit in this.suits) {
        for (const number in this.numbers) {
          deck.push({
            number: this.numbers[number],
            suit: this.suits[suit],
            name: this.numbers[number] + this.suits[suit],
            url:
              "/png/" +
              this.numbers[number] +
              "_of_" +
              this.suits[suit] +
              ".png",
          });
        }
      }
      this.deck = deck.slice(0, 10);
    },
    shuffleDeck(deck) {
      deck.sort(() => Math.random() - 0.5);
    },
    checkTrunedCards(isTurnedFaceUp) {
      if (isTurnedFaceUp) {
        this.flippedUpCards++;
      } else {
        this.flippedUpCards--;
      }
    },
  },
};
</script>