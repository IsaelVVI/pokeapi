<template>
  <div class="base d-flex justify-center">
    <v-col cols="12" md="8" sm="8" class="coluna d-flex justify-center align-start" >
      <v-card elevation="1" class="card-pokemon" >
            <v-col style="margin-bottom: 40px" cols="12" md="12" sm="12" class="d-flex justify-center flex-column">
                <v-col cols="12 d-flex">
                    <v-text-field v-model="pokemon_name" label="Pokemon" placeholder="Pikachu" dense outlined ></v-text-field>
                    <v-btn @click="searchPokemon(pokemon_name)" elevation="0" tile style="height: 40px"><v-icon>mdi-magnify</v-icon></v-btn>
                </v-col>
                <v-col v-show="pokemon != ''" cols="12" md="12" sm="12" class="d-flex justify-center">
                    <v-card v-show="pokemon != ''" class="content-pokemon" :style="{'background-color': pokemon_color}">
                        <div v-show="pokemon_color != 'white'" class="text-pokemon">
                            <b>#{{pokemon.id}}</b>
                            <h2>{{pokemon.name}}</h2>
                        </div>
                        <div v-show="pokemon_color == 'white'" class="text-pokemon" style="color: black">
                            <b>#{{pokemon.id}}</b>
                            <h2>{{pokemon.name}}</h2>
                        </div>
                        <div v-show="pokemon_color == 'yellow'" class="text-pokemon" style="color: black">
                            <b>#{{pokemon.id}}</b>
                            <h2>{{pokemon.name}}</h2>
                        </div>
                        <v-col cols="12 d-flex mt-16 align-items-center">
                            <v-img class="image-pokemon" :src="pokemon_image">
                            </v-img>
                            <div class="japanese-name">
                                <h2 class="japanese-pokemon">{{pokemon_name_japanese}}</h2>
                            </div>
                        </v-col>  
                        <v-col class="justify-end align-end">
                            <v-sheet                            
                            class="sprite-pokemon"
                            color="transparent" 
                            elevation="4"                           
                            height="90"
                            width="90"
                            >
                                <b v-show="gif_pokemon == null || gif_pokemon == undefined">no Sprite</b>
                                <v-img v-show="gif_pokemon != null && gif_pokemon != undefined " :src="gif_pokemon"> </v-img>
                            </v-sheet>                            
                        </v-col>                      
                    </v-card>
                </v-col>
                    <v-container v-if="pokemon != ''" >
                        <v-col cols="12">
                            <v-row>
                                <v-card elevation="0" class="d-flex justify-center" width="50%" v-for="(status, i) in statsfinal" :key="i">
                                    <v-col cols="12" class="d-flex justify-center flex-column align-center">
                                        <b>{{status['stat'].name}}</b>
                                            <v-progress-circular
                                                :rotate="360"
                                                :size="80"
                                                :width="5"
                                                :value="status.percentage"
                                                color="teal"
                                                >
                                                {{ status.base_stat }}
                                            </v-progress-circular>
                                    </v-col>
                                </v-card>
                            </v-row>

                        </v-col>                             
                    </v-container>
            </v-col>
      </v-card>
    </v-col>
  </div>
</template>

<script>
export default {
  data() {
    return {
        interval: {},
        statspk: null,
        statsfinal: [],
        namestats: null,
        value: null,        
        pokemon_name: '',
        pokemon_name_japanese: '',
        pokemon_image: '',
        pokemon: '',
        gif_pokemon: null,
        pokemon_color: '',
    };
  },
  created(){
    // this.statsPokemon()
  },

  methods: {
    statsPokemon(){
        const {stats} = this.pokemon
        // const {name} = stats.stat
        this.statspk = stats
        // this.namestats = name
        this.statspk.forEach(valor => {
            valor = {... valor, percentage: valor.base_stat / 2}
            this.statsfinal.push(valor)
        });
        let valor = this.statspk;
        this.value = valor / 2
        console.log(this.statsfinal);
    },
    async searchPokemon(name){
        try {
            const response = await this.axios.get(`pokemon/${name.toLowerCase()}`);
            const {sprites} = response.data
            const image = sprites['other']['official-artwork']['front_default']
            const sprite = sprites['versions']['generation-v']['black-white']['animated']['front_default']

            this.pokemon = response.data
            this.pokemon = {... response.data, name: response.data.name.charAt(0).toUpperCase() + response.data.name.slice(1)}
            this.gif_pokemon = sprite

            this.pokemon_image = image            
            // console.log(response.data);

            const species = await this.axios.get(`pokemon-species/${name.toLowerCase()}`)
            
            const {color} = species.data
            
            const {names} = species.data

            this.pokemon_color = color.name

            this.pokemon_name_japanese = names[9]['name']
            this.statsfinal = []


            // console.log(this.pokemon);
            this.statsPokemon()


        } catch (err) {
            
        }
    }
  },
};
</script>

<style scoped>
.base {
  width: 100%;
  height: 100%;
  background-color: #d1e5d9;
  background-image: url('https://ahost2.bulbagarden.net/content/bulbagarden/images/bg-green-2x-optim.png');
  
}

.coluna {
  width: 100%;
}

.card-pokemon {
  width: 100%;
  
}

@media only screen and (min-width: 600px){
    .card-pokemon{
        width: 100%;
    } 

}
@media only screen and (min-width: 768px){
    .card-pokemon{
        width: 60%;
    } 

    .image-pokemon{
    position: absolute;
    top: 20px;
    z-index: 2;
}
}

.content-pokemon {
    height: 28rem;        
    /* z-index: 1; */
    /* position: absolute; */
}

.code-pokemon{
 margin-left: 40px;
 margin-top: 40px;
}

.text-pokemon{
 left: 2rem;
 top: 2rem;
 position: absolute;
 color: aliceblue;
}

.image-pokemon{
    position: absolute;
    z-index: 2;
}



.japanese-pokemon{
    z-index: 1;
    /* margin-top: 0px; */
    /* margin-left: 140px; */
    font-size: 6rem;
    color: aliceblue;
}

.japanese-name{
    /* width: 60%; */
    height: 300px;
    overflow: hidden;
}

.sprite-pokemon{
    z-index: 999;
    bottom: 0;
    right: 0;
    position: absolute;
    background-color: rgba(255, 255, 255, 0.616);
    border: 2px solid;
    border-color: white;
}

</style>