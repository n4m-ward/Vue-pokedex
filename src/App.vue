<template>
  <div id="app">
    <div class='column is-half is-offset-one-quarter'>
      <input type="text" name="" class="input is-hovered" placeholder="Buscar Pokemon" v-model="busca">
      <button class="button is-small is-medium is-fullwidth is-dark" id="btnBusca" @click="buscar">Buscar</button>
      <div v-for="(poke,index) in filteredPokemons" :key='poke.url'>
        <Pokemon :name='poke.name' :url='poke.url' :num='index +1'/>
      </div>
  </div>
  </div>
</template>

<script>

import Pokemon from './components/Pokemon'
import axios from 'axios'

export default {
  name: 'App',
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
    
  },
  methods:{
    buscar: function(){
        this.filteredPokemons = this.pokemons;
        if(this.busca == '' || this.busca == ' ' || this.busca == undefined){
          this.filteredPokemons = this.pokemons;
        }else{
          this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
    }
    }
  },
  components:{
    Pokemon
  },
  created: function(){
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res =>{
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  /*computed:{ busca sem os botoes ser retirada por ser muito 'custosa' para um pc mais fraco, porem preferi manter no codigo
    findPokemon: function(){
      if(this.busca == '' || this.busca == ' ' || this.busca == undefined){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  }*/

}
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
#btnBusca{
  margin-top: 1%;
}
</style>
