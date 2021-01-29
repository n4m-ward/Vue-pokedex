<template>
  <div class="column is-half is-offset-one-quarter">
      <div class="basePoke">
      </div>
      <div class="tabs is-centered is-boxed is-medium">
        <ul>
            <li class="is-active" id="menu1">
                <a @click="openStatsDiv(1,'menu1')">
                    <span>Pokemon</span>
                </a>
            </li>
            <li id="menu2">
                <a @click="openStatsDiv(2,'menu2')">
                    <span>Stats</span>
                </a>
            </li>
            <li id="menu3">
                <a @click="openStatsDiv(3,'menu3')">
                    <span >Moves</span>
                </a>
            </li>
            <li id="menu4">
                <a @click="openStatsDiv(4,'menu4')">
                    <span>Documents</span>
                </a>
            </li>
        </ul>
    </div>

    <div v-if="this.pokeContainer === 1">
        
            <div class="card pokeMain">
                <div class="card-content">
                    <div class="media">
                    <div class="media-left">
                        <figure class="image is-48x48">
                        <img :src="currentImg" alt="Placeholder image">
                        </figure>
                    </div>
                    <div class="media-content">
                        <p class="title is-4">{{pokemon.name }}</p>
                        <p class="subtitle is-6">@johnsmith</p>
                    </div>
                    </div>

                    <div class="content">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                    Phasellus nec iaculis mauris. <a>@bulmaio</a>.
                    <a href="#">#css</a> <a href="#">#responsive</a>
                    <br>
                    <time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time>
                    </div>
                </div>
                </div>
            
    </div>

    <div v-else-if="this.pokeContainer === 2">
        <GChart class="chartContainer"
            type="PieChart"
            :data="chartData"
            :options="chartOptions"
            />
    </div>

    <div v-else-if="this.pokeContainer === 3">
        
        
    </div>

    <div v-else>
        <h3>{{this.pokeContainer}}</h3>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { GChart } from 'vue-google-charts'

export default {
    created:  function(){
        let url = window.location.href;
        url = url.split("/");
        const urlLenght = url.length;
        const param = url[urlLenght-1];

        axios.get(`https://pokeapi.co/api/v2/pokemon/${param}`).then(res=>{
            console.log(res.data);
            this.pokemon.abilities = res.data.abilities;
            this.pokemon.name = res.data.name;
            this.pokemon.moves = res.data.moves;
            this.currentImg = res.data.sprites.front_default;
            this.pokemon.sprites.front = res.data.sprites.front_default;

            res.data.stats.forEach(stats=>{
                const name = stats.stat.name;
                const baseStats = stats.base_stat;
                this.chartData.push([name,baseStats]);
            })
        });
        
    },
    methods:{
        openStatsDiv: function(value,id){
            let menuItem = document.getElementById(id);
            let isActive = document.getElementsByClassName('is-active')[0];
            isActive.className = '';
            menuItem.className = 'is-active';
            this.pokeContainer = value;
        }
    },
    data(){
        return{
            showLoading:true,
            pokeContainer: 1,
            isFront: true,
            currentImg: '',
            pokemon:{
                name:'',
                abilities:[],
                stats:[],
                moves:[],
                sprites:{
                    front :'',
                    back:''
                }
            },
            chartData: [
                ['Status','value']
            ],
            chartOptions: {
                chart: {
                title: 'Pokemon Stats',
                subtitle: "Status Base do Pokemon",
                }
            }
        }
    },
    components:{
        GChart
    },
    filters:{
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName
        }
    },
}
</script>

<style>
    .pokeMain{
        width:300px;
        margin: 0 auto;
    }
</style>