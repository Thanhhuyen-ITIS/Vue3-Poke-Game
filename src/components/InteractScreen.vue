<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((720 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardsContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :imgBackFaceUrl="`/images/${card}.png`"
        :ref="`card-${index}`"
        :card="{ index, value: card }"
        :cardsContext="cardsContext"
        @onFlip="checkRule($event)"
        @offFlip="off($event)"
      />
    </div>
  </div>
  ./CardC.vue
</template>

<script>
import CardFlip from "./CardC.vue";

export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value &&
        this.rules[0].index != this.rules[1].index
      ) {
        //
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
        //reser rules[]
        this.rules = [];

        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );

        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          console.log("end game");
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value != this.rules[1].value
      ) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          //reser rules[]
          this.rules = [];
        }, 800);
        //close 2 card
      } else return false;
    },
    off() {
      this.rules = [];
    },
  },
};
</script>
<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: #212121;
  color: #f3f3f3;
}
.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
