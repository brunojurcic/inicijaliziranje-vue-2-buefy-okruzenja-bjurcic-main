<template>
  <div class="step">
    <h2 class="title">Hey, hey, here's the winner of the round</h2>
    <div class="answers">
      <div
        v-for="answer in sortedAnswers"
        :key="answer.id"
        :class="{ answer: true, winner: answer.id === winnerId }"
        @click="vote(answer.id)"
      >
        {{ answer.text }} got {{ votes[answer.id] }} vote/s
      </div>
    </div>
    <b-button type="is-primary" @click="$emit('next')"
      >Go to the next question</b-button
    >
    <b-button @click="$emit('finish')">Finish the game</b-button>
  </div>
</template>
<script>
export default {
  props: {
    answers: {
      type: Object,
      required: true,
    },
    votes: {
      type: Object,
      required: true,
    },
    numberOfPlayers: {
      type: Number,
      required: true,
    },
  },
  computed: {
    sortedAnswerIds() {
      return Object.keys(this.answers).sort((a, b) => {
        return this.votes[b] - this.votes[a];
      });
    },
    sortedAnswers() {
      return this.sortedAnswerIds
        .filter((id) => id <= this.numberOfPlayers)
        .map((id) => ({ id, text: this.answers[id] }));
    },
    winnerId() {
      return this.sortedAnswerIds[0];
    },
  },
};
</script>
<style scoped>
.answer {
  padding: 1rem;
  background: #f2effb;
  font-size: 1.2rem;
  margin-bottom: 10px;
}
.answer.winner {
  background: gold;
  font-size: 1.5rem;
}
</style>
