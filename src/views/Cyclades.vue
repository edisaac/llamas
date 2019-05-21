<template>
  <span>
    <label htmlFor="tipo">Tipo : </label>
    <select v-model="tipo" @change="onChangeTipo()">
      <option v-for="item in tipos" v-bind:value="item" v-bind:key="item.id">
        {{ item.name }}
      </option>
    </select>

    <label htmlFor="jugadores">Jugadores : </label>
    <select v-model="players" @change="onChangePlayers()">
      <option v-for="n in tipo.jugadores" v-bind:value="n" v-bind:key="n"
        >{{ n }}
      </option>
    </select>
    <button @click="randomGods()" class="btn btn-default">
      Siguiente Turno
    </button>

    <br />
    <h2>Turno: {{ turno }}</h2>

    <table class="table">
      <tbody>
        <tr v-for="item in activeGods" v-bind:key="item">
          <td>
            <img v-bind:src="generarUrl(item)" width="100%" />
          </td>
        </tr>
      </tbody>
    </table>
  </span>
</template>

<script>
export default {
  name: 'home',
  data: function() {
    return {
      godurl: 'https://edisaac.github.io/gods/',
      tipo: {},
      players: 0,
      turno: 0,
      activeGods: [],
      playableGods: [],
      cant: 0,
      tipos: [
        { id: 1, name: 'Normal', jugadores: [2, 3, 4, 5] },
        { id: 2, name: 'Titans', jugadores: [2, 3, 4, 5, 6] }
      ]
    }
  },
  methods: {
    randomGods: function() {
      let firstGod = this.activeGods[0]
      let gods = this.shuffle(this.playableGods)
      if (this.cant < gods.length) {
        gods = gods.filter(god => god !== firstGod)
        gods.push(firstGod)
      }
      this.activeGods = gods.filter((god, index) => index < this.cant)
      this.turno++
    },
    generarUrl: function(god) {
      return this.godurl + god + '.png'
    },
    onChangeTipo: function() {
      if (this.tipo.name === 'Normal') {
        this.playableGods = ['ares', 'poseidon', 'athena', 'zeus']
      } else {
        this.playableGods = ['ares', 'poseidon', 'athena', 'zeus', 'kronos']
      }
      this.players = 0
      this.turno = 0
      this.activeGods = []
    },
    onChangePlayers: function() {
      this.cant = this.cantidadActiva(this.playableGods, this.players)
      this.playableGods = this.shuffle(this.playableGods)
      this.turno = 0
      this.activeGods = []
    },
    cantidadActiva: function(gods, players) {
      let cant = gods.length
      if (players === 2) {
        cant = 4
      } else {
        if (players - 1 - cant < 0) {
          cant = cant + (players - cant) - 1
        }
      }
      return cant
    },
    shuffle: function(array) {
      let currentIndex = array.length,
        temporaryValue,
        randomIndex
      while (0 !== currentIndex) {
        randomIndex = Math.floor(Math.random() * currentIndex)
        currentIndex -= 1
        temporaryValue = array[currentIndex]
        array[currentIndex] = array[randomIndex]
        array[randomIndex] = temporaryValue
      }
      return array
    }
  }
}
</script>

<style></style>
