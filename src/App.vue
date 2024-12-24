<template>
  <div id="app">
    <div class="wrapper clearfix">
      <div class="player-panel" :class="{ active: winner == 1 }">
        <div class="player-name" :class="{ isTurn: playerTurn == 1 }">{{ player[1].name }}</div>
        <div class="player-score">{{ player[1].totalScore }}</div>
        <div class="player-current-box">
          <div class="player-current-label">Current</div>
          <div class="player-current-score">{{ player[1].currentScore }}</div>
        </div>
      </div>

      <div class="player-panel" :class="{ active: winner == 2 }">
        <div class="player-name" :class="{ isTurn: playerTurn == 2 }">{{ player[2].name }}</div>
        <div class="player-score">{{ player[2].totalScore }}</div>
        <div class="player-current-box">
          <div class="player-current-label">Current</div>
          <div class="player-current-score">{{ player[2].currentScore }}</div>
        </div>
      </div>

      <button class="control btn-new" v-on:click="handleStartNewGame()"><i class="ion-ios-plus-outline"></i>New
        game</button>
      <button class="control btn-roll" v-on:click="handleRollDice()"><i class="ion-ios-loop"></i>Roll dice</button>
      <button class="control btn-hold" v-on:click="handleHoldScore()"><i
          class="ion-ios-download-outline"></i>Hold</button>

      <input v-model="finalScrore" type="number" placeholder="Final score" class="final-score">

      <div id="dice-1" class="dice">
        <div class="spinner" :class="changeDiceNumber(1)">
          <div class="face1">1</div>
          <div class="face2">2</div>
          <div class="face3">3</div>
          <div class="face4">4</div>
          <div class="face5">5</div>
          <div class="face6">6</div>
        </div>
      </div>
      <div id="dice-2" class="dice">
        <div class="spinner" :class="changeDiceNumber(2)">
          <div class="face1">1</div>
          <div class="face2">2</div>
          <div class="face3">3</div>
          <div class="face4">4</div>
          <div class="face5">5</div>
          <div class="face6">6</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      player: this.initializePlayers(),
      dice: this.initializeDices(),
      startGame: false,
      finalScrore: '',
      msgAlertFS: 'Please enter the FINAL-SCORE first!',
      msgAlertFSnNG: 'Please enter the FINAL-SCORE and click NEW-GAME to Start!',
      playerTurn: 0,
      winner: false
    }
  },
  methods: {
    initializePlayers() {
      return [
        undefined,
        {
          name: 'Player 1',
          totalScore: 0,
          currentScore: 0
        },
        {
          name: 'Player 2',
          totalScore: 0,
          currentScore: 0
        }
      ];
    },
    initializeDices() {
      return {
        one: this.randomeDiceNumber(),
        two: this.randomeDiceNumber()
      };
    },
    handleStartNewGame() {
      if (!this.finalScrore) {
        alert(this.msgAlertFS);
      } else {
        this.startGame = true;
        this.player = this.initializePlayers();
        alert("Ok, Let's Start!");
        this.playerTurn = 1;
        this.dice.one = this.dice.two = 1;
      }
    },
    handleRollDice() {
      if (this.startGame === false) {
        alert(this.msgAlertFSnNG);
      } else {
        this.dice.one = this.randomeDiceNumber();
        this.dice.two = this.randomeDiceNumber();
        setTimeout(() => {
          if (this.dice.one === 1 || this.dice.two === 1) {
            this.player[1].currentScore = 0;
            this.player[2].currentScore = 0;
            alert("Oops! You got 1, your current score is reset to 0!");
            this.playerTurn = this.changePlayerTurn();
          } else {
            this.player[this.playerTurn].currentScore += this.dice.one + this.dice.two;
          }
        }, 800);
      }
    },
    handleHoldScore() {
      if (this.startGame === false) {
        alert(this.msgAlertFSnNG);
      } else {
        this.player[this.playerTurn].totalScore += this.player[this.playerTurn].currentScore;
        this.player[this.playerTurn].currentScore = 0;
        this.playerTurn = this.changePlayerTurn();
        alert("Your score is holded! Next player's turn!");
        this.detectWinner();
      }
    },
    detectWinner() {
      if (this.player[1].totalScore >= this.finalScrore) {
        alert("Player 1 is the winner!");
        this.winner = 1;
        this.startGame = false;
      } else if (this.player[2].totalScore >= this.finalScrore) {
        alert("Player 2 is the winner!");
        this.winner = 2;
        this.startGame = false;
      }
    },
    changeDiceNumber(no) {
      return (no == 1) ? "dice-" + this.dice.one : "dice-" + this.dice.two;
    },
    randomeDiceNumber() {
      return Math.floor(Math.random() * 6) + 1;
    },
    changePlayerTurn() {
      return this.playerTurn = (this.playerTurn === 1) ? 2 : 1;
    },
  },
  computed: {

  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
