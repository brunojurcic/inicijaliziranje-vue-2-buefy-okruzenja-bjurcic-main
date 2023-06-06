<template>
  <div class="step">
    <h2 class="title">
      Current game:
      <span class="has-text-primary">{{ currentGame }}</span>
    </h2>
    <h3 class="title is-5">Players</h3>
    <div class="players">
      <div class="player" v-for="(player, id) in players" :key="id">
        <div
          class="name"
          :class="{ disabled: !player }"
          v-if="!isEdit(id)"
          @click="startEdit(id)"
        >
          {{ player ? player : "Click edit to enter name" }}
        </div>
        <div class="name" v-else>
          <b-input v-model="editName"></b-input>
          <b-button @click="save" type="is-primary" :disabled="!editName">
            Save
          </b-button>
        </div>
      </div>
    </div>
    <div class="start">
      <b-button
        @click="start"
        type="is-primary"
        :disabled="numberOfPlayers < 2"
      >
        Start ({{ numberOfPlayers }})
      </b-button>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    currentGame: {
      type: String,
      required: true,
    },
    players: {
      type: Object,
      required: true,
    },
    numberOfPlayers: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      editId: null,
      editName: "",
    };
  },
  methods: {
    isEdit(id) {
      return id === this.editId;
    },
    startEdit(id) {
      this.editId = id;
      this.editName = this.players[id];
    },
    save() {
      this.$emit("updatePlayers", {
        ...this.players,
        [this.editId]: this.editName,
      });
      this.editId = null;
    },
    start() {
      this.$emit("start");
    },
  },
};
</script>
<style>
.player {
  padding: 1rem;
  border-radius: 3px;
  margin-bottom: 0.5rem;
  background: #f2effb;
  cursor: pointer;
}
.name {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.name .control {
  flex: 1;
  margin-right: 10px;
}
.player .name.disabled {
  color: #aaa;
}
</style>
