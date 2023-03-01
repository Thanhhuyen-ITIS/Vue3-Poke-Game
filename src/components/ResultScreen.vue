<template>
  <div class="screen">
    <h3>Congratulation</h3>
    <p>{{ Math.round(timer / 1000) }} seconds</p>
    <button @click="onStartAgain">Start Again</button>
  </div>
</template>

<script>
export default {
  props: {
    timer: {
      type: Number,
      required: true,
    },
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain");
    },
  },
};
</script>
<style scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: #212121;
  color: #f3f3f3;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.screen h1 {
  font-size: 5rem;
}
.screen h3 {
  margin-top: 1.5rem;
  font-size: 3rem;
}
.screen button {
  font: "Odibee Sans", cursive;
  background: transparent;
  box-shadow: none;
  border: 1px solid #f3f3f3;
  color: #f3f3f3;
  margin: 1rem;
  padding: 1rem 1.25rem;
  border-radius: 0.5rem;
  font-size: 1.25rem;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}
.screen button:hover {
  background-color: #f3f3f3;
  color: #212121;
}
</style>
