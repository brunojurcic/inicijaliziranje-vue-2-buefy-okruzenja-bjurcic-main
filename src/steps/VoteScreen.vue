<template>
  <div class="step">
    <h2 class="title">Voting time!</h2>
    <h3 class="subtitle">
      Voting now: <strong>{{ votingPlayer }}</strong>
    </h3>
    <div class="answers">
      <div
        class="answer"
        v-for="(answer, id) in answers"
        v-show="id <= numberOfPlayers"
        :key="id"
        @click="vote(id)"
      >
        {{ answer }}
      </div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    votingPlayer: {
      type: String,
      required: true,
    },
    answers: {
      type: Object,
      required: true,
    },
    numberOfPlayers: {
      type: Number,
      required: true,
    },
  },
  methods: {
    vote(answerId) {
      this.$emit("vote", answerId);
    },
  },
};
</script>
<style scoped>
.answers {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}
.answer {
  padding: 1rem;
  background: #f2effb;
  font-size: 1.2rem;
  margin-right: 10px;
  cursor: pointer;
}
</style>
