<template>
    <div id='pokemon'>
        <div class="card is-two-fifths">
            <div class="card-image">
                <figure>
                <img :src="this.currentImg" alt="Pokemon logo">
                </figure>
            </div>
            <div class="card-content">
                <div class="media">
                <div class="media-left">
                </div>
                <div class="media-content">
                    <router-link :to="{name:'Pokemon',params:{pokemon:name}}" class="title is-4">#{{num}} - {{name | upper}}</router-link>
                    <br>
                    <p class="subtitle is-6">{{ pokemon.type }}</p>
                </div>
                </div>

                <div class="content">
                    <button class="button is-active" @click="mudarSprite">Mudar Sprite</button>
                </div>
            </div>
            </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    created: function(){
        axios.get(this.url).then(res=>{
            this.pokemon.type = res.data.types[0].type.name;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.currentImg = this.pokemon.front;
        })
    },
    data(){
        return{
            isFront: true,
            currentImg: '',
            pokemon : {
                type: '',
                front: '',
                back: ''
            }
        }
    },
    props:{
        num: Number,
        name: String,
        url: String
    },
    filters:{
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName
        }
    },
    methods:{
        mudarSprite: function(){
            if(this.isFront){
                this.currentImg = this.pokemon.back;
                this.isFront = false;
            }else{
                this.currentImg = this.pokemon.front;
                this.isFront = true;
            }
        }
    }
}
</script>

<style scoped>
    #pokemon{
        margin:0 auto;
        margin-top:2%;
        align-content: center;
        justify-content: center;
    }
    .card{
        float:left;
        width:250px;
        margin:20px;
    }
</style>