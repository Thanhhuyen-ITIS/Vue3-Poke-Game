<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
  <copy-right-screen />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue";
import CopyRightScreen from "./components/CopyRightScreen.vue";

export default {
  name: "App",
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRightScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffled(shuffled(shuffled(cards)));

      this.settings.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },

    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
    },
  },
};
</script>
