<template>
  <div class="simon-game">
    <div class="container">
      <div class="wrapper">
        <div class="simon">
          <div class="simon-board">
            <div
              class="red simon-card"
              ref="1"
              @click="pressBtn(1)"
            ></div>
            <div
              class="blue simon-card"
              ref="2"
              @click="pressBtn(2)"
            ></div>
            <div
              class="yellow simon-card"
              ref="3"
              @click="pressBtn(3)"
            ></div>
            <div
              class="green simon-card"
              ref="4"
              @click="pressBtn(4)"
            ></div>
          </div>
          <button
            class="simon-btn"
            @click="start"
          >
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
        levelTime: {
          Легкий: "1500",
          Средний: "1000",
          Сложный: "400",
        },
        audio: {
          1: new Audio("https://s3.amazonaws.com/freecodecamp/simonSound1.mp3"),
          2: new Audio("https://s3.amazonaws.com/freecodecamp/simonSound2.mp3"),
          3: new Audio("https://s3.amazonaws.com/freecodecamp/simonSound3.mp3"),
          4: new Audio("https://s3.amazonaws.com/freecodecamp/simonSound4.mp3"),
        },
        computerSequence: [],
        userSequence: [],
        timeoutID: null,
        time: 1500,
        inGame: false,
      };
    },
    methods: {
      pressBtn(id) {
        this.audio[id].play();
        this.$refs[id].style.opacity = "1";
        setTimeout(() => {
          this.$refs[id].style.opacity = "0.5";
        }, 100);
        if (this.inGame) {
          clearTimeout(this.timeoutID);
          this.userSequence.push(id);
          this.checkUserSequence(this.userSequence.length - 1);
        }
      },
      start() {
        this.computerSequence = [];
        this.userSequence = [];
        this.time = this.levelTime[this.level];
        this.step();
      },

      step() {
        this.userSequence = [];
        this.playComputerSequence();
        this.buttonText = "Слушайте";
      },
      random() {
        const num = Math.floor(Math.random() * 4) + 1;
        return Math.floor(num);
      },
      playComputerSequence() {
        this.inGame = false;
        this.computerSequence.push(this.random());

        this.computerSequence.forEach((id, i) => {
          setTimeout(() => {
            this.pressBtn(id);
          }, 700 * i);
        });
        setTimeout(() => {
          this.inGame = true;
          this.buttonText = "Повторите";
          this.timer();
        }, this.computerSequence.length * 700);
      },
      timer() {
        this.timeoutID = setTimeout(() => {
          this.gameOver();
        }, this.time);
      },
      checkUserSequence(item) {
        if (this.userSequence[item] !== this.computerSequence[item]) {
          return this.gameOver();
        }
        if (this.userSequence.length === this.computerSequence.length) {
          this.round++;
          setTimeout(() => {
            this.step();
          }, 1000);
        } else {
          this.timer();
        }
      },
      gameOver() {
        this.buttonText = "Game Over";
        setTimeout(() => {
          this.buttonText = "Старт";
        }, 1000);
        clearTimeout(this.timeoutID);
        this.round = 0;
        this.inGame = false;
        this.userSequence = [];
        this.computerSequence = [];
      },
    },
  };
</script>

<style lang="scss">
  @import "./styles/main.sass";
</style>
