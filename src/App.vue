<template>
  <div id="app">
    <HomeScreen v-if="step === steps.HOME" @join="join" @create="create" />
    <LobbyScreen
      v-else-if="step === steps.LOBBY"
      :currentGame="currentGame"
      :players="players"
      :numberOfPlayers="numberOfPlayers"
      @updatePlayers="updatePlayers"
      @start="startAnswering"
    />
    <AnswerScreen
      v-else-if="step === steps.ANSWER"
      :answeringPlayer="player"
      :currentQuestion="currentQuestion"
      :numberOfPlayers="numberOfPlayers"
      @saveAnswer="saveAnswer"
    />
    <VoteScreen
      v-else-if="step === steps.VOTE"
      :votingPlayer="player"
      :answers="answers"
      :numberOfPlayers="numberOfPlayers"
      @vote="vote"
    />
    <ResultScreen
      v-else-if="step === steps.RESULT"
      :answers="answers"
      :votes="votes"
      :numberOfPlayers="numberOfPlayers"
      @next="startAnswering"
      @finish="finishGame"
    />
  </div>
</template>

<script>
import questions from "@/assets/questions.json";
import { getRandomInt } from "@/helpers";
import HomeScreen from "@/steps/HomeScreen.vue";
import LobbyScreen from "@/steps/LobbyScreen.vue";
import AnswerScreen from "@/steps/AnswerScreen.vue";
import VoteScreen from "@/steps/VoteScreen.vue";
import ResultScreen from "@/steps/ResultScreen.vue";

const steps = {
  HOME: 1,
  LOBBY: 2,
  ANSWER: 3,
  VOTE: 4,
  RESULT: 5,
};

export default {
  name: "App",
  data() {
    return {
      step: steps.HOME,
      steps: steps,
      currentGame: null,
      players: null,
      currentPlayer: null,
      currentQuestion: null,
      answers: null,
      votes: null,
    };
  },
  methods: {
    join(pin) {
      this.currentGame = pin;
      this.startLobby();
    },
    create() {
      this.currentGame = getRandomInt(1000, 9999);
      this.startLobby();
    },
    startLobby() {
      this.players = {
        1: "Admin",
        2: "",
        3: "",
        4: "",
      };
      this.step = steps.LOBBY;
    },
    updatePlayers(players) {
      this.players = players;
    },
    startAnswering() {
      this.currentPlayer = 1;
      this.answers = {
        1: "",
        2: "",
        3: "",
        4: "",
      };
      this.nextQuestion();
      this.step = steps.ANSWER;
    },
    nextQuestion() {
      this.currentQuestion = questions[getRandomInt(0, questions.length - 1)];
    },
    saveAnswer(answer) {
      this.answers[this.currentPlayer] = answer;
      // Last player
      if (this.currentPlayer === this.numberOfPlayers) {
        this.startVoting();
        return;
      }
      this.currentPlayer++;
    },
    startVoting() {
      this.currentPlayer = 1;
      this.votes = {
        1: 0,
        2: 0,
        3: 0,
        4: 0,
      };
      this.step = steps.VOTE;
    },
    vote(id) {
      this.votes[id] += 1;
      if (this.currentPlayer === this.numberOfPlayers) {
        this.showResults();
        return;
      }
      this.currentPlayer++;
    },
    showResults() {
      this.step = steps.RESULT;
    },
    finishGame() {
      this.step = steps.HOME;
    },
  },
  computed: {
    player() {
      if (!this.players || !this.currentPlayer) {
        return null;
      }
      return this.players[this.currentPlayer];
    },
    numberOfPlayers() {
      return Object.values(this.players).filter((player) => !!player).length;
    },
  },
  components: {
    HomeScreen,
    LobbyScreen,
    AnswerScreen,
    ResultScreen,
    VoteScreen,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  max-width: 600px;
  margin: 0 auto;
}
.step {
  padding: 2rem;
}
</style>
