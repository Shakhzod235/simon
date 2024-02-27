<template>
  <div class="simon-game">
    <div class="container">
      <div class="wrapper">
        <div class="simon">
          <div class="simon-board">
            <div class="red simon-card"></div>
            <div class="blue simon-card"></div>
            <div class="yellow simon-card"></div>
            <div class="green simon-card"></div>
          </div>
          <button class="simon-btn">
            {{ buttonText }}
          </button>
        </div>

        <div class="setting">
          <p>Раунд: {{ round }}</p>
          <p>Уровень сложности</p>
          <label
            v-for="item in levels"
            :key="item"
          >
            <input
              type="radio"
              v-model="level"
              name="level"
              :value="item"
            />
            {{ item }}
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "simon-the-game",
    data() {
      return {
        round: 0,
        level: "Легкий",
        levels: ["Легкий", "Средний", "Сложный"],
        buttonText: "Играть",
      };
    },
    methods: {},
  };
</script>

<style lang="scss">
  @import "./styles/main.sass";
</style>
