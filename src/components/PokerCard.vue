<template>
  <span @click="turnCard" class="m-2">
    <transition name="fade" mode="out-in">
      <img v-if="faceDown" src="../assets/png/back.png" alt="card" width="100" />
      <img v-else :src="cardImage" alt="back-card" width="100" />
    </transition>
  </span>
</template>

<script>
export default {
      emits: ['turnedCard'],
      props: {
        cardImage: {
            type: String,
            required: true,
        },
        cardName: {
            type: String,
            required: true,
        },
    },
    data() {
        return {
            faceDown: true,
        };
    },
    methods: {
        turnCard() {
            this.faceDown = !this.faceDown;
            this.$emit('turnedCard', this.cardName)
        }
    },
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>