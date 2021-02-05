<template>
    <div v-if="notYetFiveCardsFlippedUp" >
  <poker-card @turned-card="toggleFlippedCard"
    v-for="card in deck"
    :card-image="card.url"
    :card-name="card.name"
    :key="card.name"
  ></poker-card>
  </div>
  <h1 v-else> Selected cards: {{flippedUpCards}}</h1>
</template>

<script>
import PokerCard from "./PokerCard.vue";
export default {
  emits: ['finishedGame'],
  components: {
    PokerCard,
  },
  data() {
    return {
      flippedUpCards: [],
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
    this.deck = this.deck.slice(0, 10)
  },
  computed: {
      notYetFiveCardsFlippedUp() {
          return this.flippedUpCards.length != 5
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
      this.deck = deck
    },
    shuffleDeck(deck) {
      deck.sort(() => Math.random() - 0.5);
    },
    toggleFlippedCard(flippedCard) {

      console.log('flippedCard', flippedCard)
      const isAlreadyCardFlipped = this.flippedUpCards.findIndex(cardName => {
        return cardName == flippedCard
      })

      console.log(isAlreadyCardFlipped)

      if (isAlreadyCardFlipped==-1) {
        this.flippedUpCards.push(flippedCard)
      }

      else {
         this.flippedUpCards.splice(isAlreadyCardFlipped, 1)
      }

      console.log(this.flippedUpCards)
      
      if (this.flippedUpCards.length==5) {
        this.$emit("finishedGame", this.flippedUpCards)
      }
    },
  },
};
</script>