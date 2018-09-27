<template>
  <div id="app">
    <section class="row">
      <div class="small-6 columns">
        <h1 class="text-center">PLAYER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;"
               :style="{width: playerWins + '0%'}">
            {{playerWins}}
          </div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">COMPUTER</h1>
        <div class="wins">
          <div class="wins text-center" style="background-color: green; margin: 0; color: white;"
               :style="{width: computerWins + '0%'}">
            {{computerWins}}
          </div>
        </div>
      </div>
    </section>
    <section class="row controls" v-if="!show">
      <div class="small-12 columns">
        <button id="start-game" @click="startGame">START NEW GAME</button>
      </div>
    </section>
    <section class="row controls" v-else>
      <div class="small-12 columns">
        <button id="rock" @click="userRock">ROCK</button>
        <button id="paper" @click="userPaper">PAPER</button>
        <button id="scissors" @click="userScissors">SCISSORS</button>
        <button id="restart" @click="startGame">RESTART</button>
      </div>
    </section>
    <section class="row log" v-show="show">
      <div class="small-12 columns">
        <ul>
          <li v-for="turn in turns" :class="{'player-turn':turn.isPlayer, 'computer-turn': !turn.isPlayer, 'tie-turn': turn.tie}">
            {{ turn.text }}
          </li>
        </ul>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      playerWins: 0,
      computerWins: 0,
      show: false,
      rock: 1,
      paper: 2,
      scissors: 3,
      turns: []
    }
    },
      watch: {
      playerWins: function () {
        this.checkWinner();
      },
      computerWins: function() {
        this.checkWinner();
      }
    },
    methods: {
      compChoice: function () {
        let max = 3;
        let min = 0;

        return Math.max(Math.floor(Math.random() * max) + 1, min)

      },

      startGame: function () {
        this.show = true;
        this.playerWins = 0;
        this.computerWins = 0;
        this.turns = [];
      },

      userRock: function () {
        //generate computer choice
        let computerChoice = this.compChoice();
        console.log(computerChoice);
        //output winner
        if (computerChoice == this.paper) {
          console.log("computerWins");
          this.computerWins += 1;
          //check for winner
          this.turns.unshift({
            isPlayer: false,
            text: 'Computer Chose Paper ! Paper Beats Rock ! Computer Wins'
          })
        }
        else if (computerChoice == this.scissors) {
          console.log("playerWins");
          this.playerWins += 1;
          //check for winner
          this.turns.unshift({
            isPlayer: true,
            text: 'Computer Chose Scissors ! Rock Beats Scissors ! You Win'
          })
        }
        else {
          //check for winner
          console.log("tie");
          this.turns.unshift({
            tie: true,
            text: 'Computer chose rock, you chose rock! Its a Tie!'
          })
        }
      },
      userPaper: function () {
        //generate computer choice. let computerChoice is making a variable that generates random #
        let computerChoice = this.compChoice();
        //output winner by adding 1 to the winning player +=
        if (computerChoice == this.rock) {
          console.log("playerWins");
          this.playerWins += 1;
          //check for winner
          this.turns.unshift({
            isPlayer: true,
            text: 'Computer chose rock! Paper beats rock! You Win!!'
          })
        }
        else if (computerChoice == this.scissors) {
          console.log("computerWins");
          this.computerWins += 1;
          //check for winner
          this.turns.unshift({
            isPlayer: false,
            text: 'Computer chose scissors! Scissors beats paper! Sorry, you lose! '
          })
        }
        else {
          console.log("tie");
          this.turns.unshift({
            tie: true,
            text: 'Computer chose paper, you chose paper! Its a Tie!! '
          })
        }
      },
      userScissors: function () {
        //generate computer choice random number.
        let computerChoice = this.compChoice();
        //output winner by adding 1 to winning player +=
        if (computerChoice == this.rock) {
          console.log("computerWins");
          this.computerWins += 1;
          this.turns.unshift({
            isPlayer: false,
            text: 'Computer chose rock! Rock beats Scissors! Sorry, you lose'
          })
        }

        else if (computerChoice == this.paper) {
          console.log("playerWins");
          this.playerWins += 1;
          this.turns.unshift({
            isPlayer: true,
            text: 'Computer chose paper! Scissors beats paper! You win!'
          })
        }

        else {
          console.log("tie");
          this.turns.unshift({
            tie: true,
            text: 'Computer chose Scissors, you chose Scissors! Its a tie! '
          })
        }
      },


      checkWinner: function () {
        if (this.playerWins == 10) {
          if (confirm('You won! New Game.')) {
            this.startGame();
          }
          else {
            this.gameIsRunning = false;
          }
        }
        else if (this.computerWins == 10) {
          if (confirm('You lost! New game.')) {
            this.startGame();
          }

        }
        else{
          console.log("Games not over")
        }
      }

    }
  }

</script>

<style>
  .text-center {
    text-align: center;
  }

  .wins {
    width: 80%;
    color: black;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 1000ms;
  }

  .controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
  }

  .turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
  }

  .log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
  }

  .log ul li {
    margin: 5px;
  }

  .log ul .player-turn {
    color: green;
    background-color: #aaffb0;
  }

  .log ul .computer-turn {
    color: red;
    background-color: #ffc0c1;
  }

  .log ul .tie-turn {
    color: blue;
    background-color: #e4e8ff;
  }

  button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
  }

  #start-game {
    background-color: #e4e8ff;
  }

  #start-game:hover {
    background-color: #687eff;
  }

  #rock {
    background-color: #ff7367;
  }

  #rock:hover {
    background-color: #ff3f43;
  }

  #paper {
    background-color: #ffaf4f;
  }

  #paper:hover {
    background-color: #ff9a2b;
  }

  #scissors {
    background-color: #aaffb0;
  }

  #scissors:hover {
    background-color: #76ff7e;
  }

  #restart {
    background-color: #ffffff;
  }

  #restart:hover {
    background-color: #c7c7c7;
  }
</style>
