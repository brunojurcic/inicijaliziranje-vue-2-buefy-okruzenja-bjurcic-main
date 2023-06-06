<template>
  <div class="step">
    <h2 class="title">Answering time!</h2>
    <h3 class="subtitle">
      Answering now: <strong>{{ answeringPlayer }}</strong>
    </h3>
    <div class="question">{{ currentQuestion.prompt }}</div>
    <div class="answer">
      <b-input v-model="answer"></b-input>
      <b-button @click="saveAnswer" type="is-primary" :disabled="!answer">Answer</b-button>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    answeringPlayer: {
      type: String,
      required: true,
    },
    currentQuestion: {
      type: String,
      required: true,
    },
    numberOfPlayers: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      answer: "",
    };
  },
  methods: {
    saveAnswer() {
      this.$emit("saveAnswer", this.answer);
      this.answer = "";
    },
  },
};
</script>
<style scoped>
.question {
  padding: 0 0 1rem;
}
.answer {
  padding: 1rem 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.answer .control {
  width: 100%;
  margin-right: 10px;
}
</style>
