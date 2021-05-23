<template>
  <div id="app">
    <img
      id="pokemonLogo"
      src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/98/International_Pok%C3%A9mon_logo.svg/1200px-International_Pok%C3%A9mon_logo.svg.png"
      alt="logoPokemon"
    />
    <h1>PokeGuía</h1>
    Nombre o ID: <input v-model="nombre" type="text" />
    <button @click="changePokemon(nombre)">Buscar</button><br />
    <img id="spritePokemon" :src="imagen" alt="imagenPokemon" />
    <h2>Movimientos</h2>
    <p v-for="(movimiento, i) in movimientos" :key="i">
      {{ movimiento.move.name }}
    </p>
    <h2>Habilidades</h2>
    <p v-for="(habilidad, i) in habilidades" :key="i">
      {{ habilidad.ability.name }}
    </p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      nombre: "pikachu",
      pokemon: {},
      imagen: "",
      movimientos: [],
      habilidades: [],
    };
  },
  methods: {
    async changePokemon(nombre) {
      try {
        const { data } = await axios.get(
          `https://pokeapi.co/api/v2/pokemon/${nombre}`
        );
        this.imagen = data.sprites.front_default;
        this.movimientos = data.moves;
        this.habilidades = data.abilities;
        this.nombre = "";
      } catch (error) {
        alert("El nombre o ID ingresado no existe");
      }
    },
  },
  created() {
    this.changePokemon(this.nombre);
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#pokemonLogo {
  width: 400px;
  height: auto;
  margin-bottom: 15px;
}

#spritePokemon {
  margin-top: 15px;
}

p {
  margin: 0;
}
</style>
