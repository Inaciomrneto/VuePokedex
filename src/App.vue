<template>
  <!-- lista e campo de busca -->
  <v-app>
    <v-container>
      <v-container>
        <v-row>
          <v-container>
            <h1 class="text-center white--text mb-8" style="font-size= 4rem">Pokedex 1ª geração</h1>
          </v-container>
        </v-row>
      </v-container>
      <v-container>
        <v-card>
          <v-text-field v-model="search" placeholder="Digite o nome do pokémon" class="input"></v-text-field>
        </v-card>
        <v-row class="space">
          <v-col cols="3" v-for="pokemon in searchPokemons" :key="pokemon.id">
            <v-card @click="showPokemon(getId(pokemon))">
              <v-container>
                <v-row class="mx-0 d-flex justify-center">
                  <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getId(pokemon)}.png`" :alt="pokemon.name" width="80%">
                </v-row>
                <h2 class="text-center">{{`#${getId(pokemon)} ` + getName(pokemon)}}</h2>
              </v-container>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-container>
    
    <!-- detalhes --> 

    <v-dialog
      v-model="showDioalog"
      width="800"
    >
      <v-card v-if="selectedPokemon" class="px-6">
        <v-container>
          <v-row class="d-flex align-center">
            <v-col cols="4">
              <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${selectedPokemon.id}.png`" :alt="selectedPokemon.name" width="80%">
            </v-col>
            <v-col cols="8">
              <h1>{{getName(selectedPokemon)}}</h1>
              <LabelTypeColor v-for="type in selectedPokemon.types" :key="type.slot" class="mr-2" :typePoke=type.type.name></LabelTypeColor>
              <v-divider class="my-4"></v-divider>
              <v-chip label>Altura: {{ selectedPokemon.height * 2.54 }} cm</v-chip>
              <v-chip label class="ml-2" >Peso: {{ (selectedPokemon.weight * 0.45359237).toFixed(0) }} Kgs</v-chip>
            </v-col>
          </v-row>
          <h2>Moves</h2>
          <v-simple-table>
            <template>
              <thead>
                <tr>
                  <th class="text-left">Nível</th>
                  <th class="text-left">Nome</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in filterMoves(selectedPokemon)" :key="item.move.name">
                  <td>{{getMoveLevel(item)}}</td>
                  <td>{{ item.move.name }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-container>
      </v-card>
    </v-dialog>
  
  </v-app>
</template>

<script>
import axios from 'axios';
import labelTypeColor from './components/LabelTypeColor.vue';
export default {
  name: 'App',

  components: {
    labelTypeColor,
  },

  data() {
    return{
      pokemons: [],
      search: '',
      showDioalog: false,
      selectedPokemon: null,
    }
  },

  mounted(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151").then((response) => { 
      this.pokemons = response.data.results;
    })
  },

  methods: {
    getId(pokemon){
      return Number(pokemon.url.split("/")[6]);
    },
    getName(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
    showPokemon(id){
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => { 
        this.selectedPokemon = response.data;
        this.showDioalog = !this.showDioalog;
      })
    },
    getMoveLevel(move){
      for(let version of move.version_group_details){
        if(version.version_group.name == "red-blue" && version.move_learn_method.name == "level-up"){
          return version.level_learned_at;
        }
      }
      return 0;
    },
    filterMoves(pokemon){
      return pokemon.moves.filter((item) => {
        let include = false;
        for(let version of item.version_group_details){
          if(version.version_group.name == "red-blue" && version.move_learn_method.name == "level-up"){
            include = true;
          }
        } 
        return include;
      })
    }
  },

  computed: {
    searchPokemons(){
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      })
    },
  }
};
</script>

<style>
  #app{
    background: linear-gradient(to bottom right,rgba(12,39,63,1), rgb(47, 107, 160)) no-repeat center center fixed;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
    background-position: center;
    min-height: 100vh;
    }
    .input{
      margin: 30px;
    }
    .space{
      padding-top: 15px;
    }
</style>