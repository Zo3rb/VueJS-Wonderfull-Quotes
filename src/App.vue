<template>
  <div id="app">
    <div class="mt-3 pb-3 text-center">
      <h1 class="border-bottom border-white d-inline">The Monster Slayer Game</h1>
      <p class="text-muted">Better Code & Structure 😜</p>
    </div>
    <div class="container-fluid">
      <div class="row py-2">
        <!-- The Player & Monster Health Bars -->
        <div class="col-12">
          <div class="row border border-white py-5 text-center">
            <div class="col-12 col-sm-6 p-1">
              <h5>Player</h5>
              <div class="progress">
                <div
                  class="progress-bar bg-primary"
                  role="progressbar"
                  :style="{width: playerHealth + '%'}"
                  aria-valuenow="25"
                  aria-valuemin="0"
                  aria-valuemax="100"
                >{{playerHealth}}</div>
              </div>
            </div>
            <div class="col-12 col-sm-6 p-1">
              <h5>Monster</h5>
              <div class="progress">
                <div
                  class="progress-bar bg-primary"
                  role="progressbar"
                  :style="{width: monsterHealth + '%'}"
                  aria-valuenow="25"
                  aria-valuemin="0"
                  aria-valuemax="100"
                >{{monsterHealth}}</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Template Will Show If The Game is Not Running -->
        <template v-if="!gameIsRunning">
          <div class="col-sm-12 border border-white p-3 d-flex justify-content-center my-5">
            <button class="btn btn-success px-5" @click="startGame">Start Game</button>
          </div>
        </template>

        <!-- Template Will Show If The Game is Running -->
        <template v-else>
          <div class="col-sm-12 border border-white p-2 d-flex justify-content-center my-3">
            <button class="btn btn-primary px-5 mx-2" @click="attackMethod">Attack</button>
            <button class="btn btn-warning px-5 mx-2" @click="specialAttackMethod">Special Attack</button>
            <button class="btn btn-success px-5 mx-2" @click="healMethod">Heal</button>
            <button class="btn btn-danger px-5 mx-2" @click="giveUpMethod">Give Up</button>
          </div>
        </template>

        <!-- Logs Will Be Shown Once The Game Starts -->
        <template v-if="logs.length">
          <div class="col-sm-12 border border-white logs">
            <ul class="list-unstyled text-center">
              <li
                v-for="(log, key, index) in logs"
                :key="index"
                :class="log.turn === 'monster' ? 'bg-danger' : 'bg-primary'"
                class="rounded my-1"
              >{{ log.turn }} hits {{ log.dmg }}</li>
            </ul>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  // Declaring Our Porject States
  data() {
    return {
      gameIsRunning: false,
      playerHealth: 100,
      monsterHealth: 100,
      logs: [],
    };
  },
  // Declaring Our Project Methods
  methods: {
    /*
    Start Game Function That will Play on The Game For Us
    return States Overwrites
    */
    startGame: function () {
      this.gameIsRunning = true;
      this.playerHealth = 100;
      this.monsterHealth = 100;
      this.logs.length = 0;
    },
    /*
    Give Up Method That Will end The Current Process and Start Ask if Want to Start New One
    return User Confirmation to Play again
    */
    giveUpMethod: function () {
      this.gameIsRunning = false;
      this.logs.length = 0;
    },
    /*
    Calcuate DMG That return Random Generated Damage
    @Param {min}: Number => minimual damdge can be generated
    @Param {Max}: Number => Maximum damdge can be generated
    return {damdge} : Number (Random Number Between Min and Max Property)
    */
    calculateDmg: function (min, max) {
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    /*
    Player Attacks Method That Decrease From Monster Health
    */
    playerAttacks: function (min, max) {
      const dmg = this.calculateDmg(min, max);
      this.monsterHealth -= dmg;
      this.logs.unshift({ turn: "Player", dmg });
    },
    /*
    Player Attacks Method That Decrease From Monster Health
    */
    monsterAttacks: function (min, max) {
      const dmg = this.calculateDmg(min, max);
      this.playerHealth -= dmg;
      this.logs.unshift({ turn: "monster", dmg });
    },
    /*
    Attack Method That Uses Player / Monster Attacks and Watching for First to Die
    */
    monsterWins: function () {
      this.playerHealth = 0;
      if (confirm("Monster Won, Play Again ? ")) this.startGame();
      this.gameIsRunning = false;
    },
    playerWins: function () {
      this.monsterHealth = 0;
      if (confirm("Player Won, Play Again ? ")) this.startGame();
      this.gameIsRunning = false;
    },
    attackMethod: function () {
      this.playerAttacks(3, 10);
      this.monsterAttacks(5, 12);
      if (this.playerHealth <= 0) {
        this.monsterWins();
      }
      if (this.monsterHealth <= 0) {
        this.playerWins();
      }
    },
    specialAttackMethod: function () {
      this.playerAttacks(6, 15);
      this.monsterAttacks(4, 14);
      if (this.playerHealth <= 0) {
        this.monsterWins();
      }
      if (this.monsterHealth <= 0) {
        this.playerWins();
      }
    },
    healMethod: function () {
      if (this.playerHealth < 90) {
        this.playerHealth += 10;
        this.monsterAttacks(3, 10);
        return;
      }
      this.monsterAttacks(5, 25);
    },
  },
};
</script>

<style>
body {
  background-color: black;
  color: white;
}
.logs {
  max-height: 200px;
  overflow-y: scroll;
}
#app {
  font-family: "Ubuntu", Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
