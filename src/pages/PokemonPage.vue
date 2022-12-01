<template>

  <h1>Quien es ese pokemon?</h1>
  <h1 v-if="!pokemon"> Espere... </h1>
  <div v-else>    
    <PokemonPicture 
      :pokemonId="pokemon.id"
      :showPokemon="showPokemon"
    />
    <PokemonOptions :pokemons="listPokemons"
                    @selection="checkAnswer"
    />
    <template v-if="showAnswer">
      <h2 class="fade-in">{{ message }}</h2>
      <button @click="newGame">
          New game
      </button>
    </template>

  </div>

  
</template>

<script>
import PokemonPicture     from '@/components/pokemon-picture/PokemonPicture.vue'
import PokemonOptions     from '@/components/pokemon-options/PokemonOptions.vue'
import getPokemonsOptions from '@/helpers/getPokemonsOptions'


export default {

    components:{
      
        PokemonPicture,
        PokemonOptions
    },

    data: ()=>({

      listPokemons:[],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: null,

      
    }),
    methods:{

      async getPokemonList(){
        
        this.listPokemons = await getPokemonsOptions()
        const rnd =  Math.floor( Math.random() * 4 )
        this.pokemon =  this.listPokemons[rnd]
        console.log(this.pokemon)
      },

      checkAnswer(selectedId){
        this.showPokemon = true
        this.showAnswer  = true
        if(selectedId === this.pokemon.id){
          this.message = `correcto! ${this.pokemon.name}`
        }else{
          this.message = `opps! era ${this.pokemon.name}`
        }
      },

      async newGame(){
        this.pokemon= null
        await this.getPokemonList()
        this.showPokemon = false
        this.showAnswer  = false

      }


    },
     mounted(){
       this.getPokemonList()
    }
}
</script>
