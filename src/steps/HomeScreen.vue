<template>
  <div class="step">
    <h2 class="title">Welcome to the game!</h2>
    <p>
      This is a simple game of writing interesting answers to interesting
      questions.
    </p>
    <div class="pin-field">
      <b-field label="Pin code">
        <b-input v-model="pin"></b-input>
      </b-field>
      <div class="join">
        <b-button @click="join" type="is-primary">Join the game</b-button>
      </div>
    </div>
    <div class="create">
      <p>You can also start a new game.</p>
      <b-button @click="create" type="is-primary is-light">
        Create new game
      </b-button>
    </div>
  </div>
</template>
<script>
export default {
  name: "HomeScreen",
  data() {
    return {
      pin: "",
    };
  },
  methods: {
    join() {
      this.$emit("join", this.pin);
    },
    create() {
      this.$emit("create");
    },
  },
};
</script>
<style scoped>
.pin-field {
  margin: 1rem 0;
  background: #f2effb;
  padding: 1rem;
  border-radius: 3px;
}
</style>
