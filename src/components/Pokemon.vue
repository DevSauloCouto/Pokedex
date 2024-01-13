<template>
  <div id="pokemon">
    
    <div class="card">
        <div class="card-image">
            <figure>
                <img :src="currentImg" :alt="name">
            </figure>
        </div>
        <div class="card-content">
            <div class="media">
                <div class="media-content">
                    <p class="title is-4">{{ idx }} - {{ upperFirstLetterName }}</p>
                    <p class="subtitle is-6">{{ pokemon.type }}</p>
                </div>
            </div>
        </div>

        <div class="content">
            <button class="button is-normal is-link is-rounded is-outlined" id="btn-change" @click="changeImg">Mudar sprite</button>
        </div>

    </div>

  </div>
</template>

<script>

import axios from "axios"

export default {
    async created(){
        try {
            const pokeResponse = await axios.get(this.url);

            this.pokemon.type = pokeResponse.data.types[0].type.name;
            this.pokemon.experience = pokeResponse.data.base_experience;
            this.pokemon.abilities = pokeResponse.data.abilities.map(abilitie => abilitie.ability.name);
            this.pokemon.imgFront = pokeResponse.data.sprites.front_default;
            this.pokemon.imgBack = pokeResponse.data.sprites.back_default;

            this.currentImg = this.pokemon.imgFront;

        } catch (e) {
            console.log(e);
        }
    },

    data(){
        return {
            isFront: true,
            currentImg: '',
            pokemon: {
                type: '',
                experience: '',
                abilities: '',
                imgFront: '',
                imgBack: '' 
            }
        }
    },

    props: {
        name: String,
        url: String,
        idx: Number
    },

    methods: {       
        changeImg() {
            if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.imgBack;
            } else {
                this.isFront = true;
                this.currentImg = this.pokemon.imgFront
            }
        }
    },

    computed: {
        upperFirstLetterName() {
            return this.name[0].toUpperCase() + this.name.slice(1);
        }
    }
}
</script>

<style>

    #pokemon {
        margin-top: 2%;
    }

    #btn-change {
        margin-bottom: 2%;
    }

</style>