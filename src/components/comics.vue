<template>
    <div class="comics">
        <div class="navbar-home">
            <input type="text"  class="search" @keyup.enter = "getComics()" 
            v-model= "deseado" placeholder="Search">
        </div>
        <h2 class="error" v-if="error.status">{{error.message}}</h2>
        <div v-else class="contenedor contenedor-fichas">
            <div class="ficha" v-for = "libro in novelasgraficas" :key= "libro.id">
            <!-- /* Se emplea el router para que al clicar se abra la vista de la ficha comic 
            y se le pasa la id que le corresponde al comic seleccionado*/ -->
                <router-link :to= "{ name: 'comic', params : {id: libro.id} }">
                    <img :src= "`${libro.thumbnail.path}/portrait_xlarge.jpg`" alt="">
                </router-link> 
                <div class="titulo-comic">
                    <p>{{libro.title}} </p>
                </div>
            </div> 
        </div>
        <Pagination @page="setPage" :limitOfSet="url.limit" :limitOfPage="10" :total="url.total" v-if="!error.status"/>
    </div>
</template>


<script>
import {publicKey, hash, timeset} from '../keys';
import axios from 'axios';
import Pagination from './Pagination';

export default {
 name: "comics",

    data(){
        return{
            novelasgraficas: [],
            deseado: '',
            url: {
               urlBase: 'https://gateway.marvel.com:443/v1/public/comics?',
               page: 0,
               limit: 10,
               total: 0
            },
            error: {
                status: false,
                message: ''
            },
        }
    },
    computed:{
        offset() {
            return  this.url.page*10
        },
        urlValidated() {
            if(this.deseado) {
                return  `${this.url.urlBase}limit=${this.url.limit}&offset=${this.offset}&titleStartsWith=${this.deseado}&ts=${timeset}&apikey=${publicKey}&hash=${hash}`
            }else {
                return  `${this.url.urlBase}limit=${this.url.limit}&offset=${this.offset}&titleStartsWith=avengers&ts=${timeset}&apikey=${publicKey}&hash=${hash}`
            }
        }
    },
    methods: {
        getComics() {
            axios.get(this.urlValidated)
            .then((result) =>{
                this.url.page = 0;
                this.error.status =false;
                this.novelasgraficas = [];
                this.url.total = result.data.data.total
                result.data.data.results.forEach(item => { 
                    this.novelasgraficas.push(item);
                })
            }).catch(() => {
                this.error.status = true;
                this.error.message = 'Sorry. SHIELD is under attack right now. Try again later';
            })
        },
        setPage(page) {
            this.url.page = page;
            this.getComics();
        },
    },
    mounted() {
        this.getComics()
    },
    components: {
        Pagination
    }
}
</script>

<style>

.contenedor{ 
    width: 100%;
    padding: 10px 0px 10px;
    display: flex;
    flex-wrap: wrap
}

.ficha{
    width: calc(20% - 20px);
    margin: 30px 10px 10px;
}

.titulo-comic{
    size: 14px;
    font-weight: bold;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    height: 72px;
    color: rgb(233, 233, 193)
}

.navbar-home{
     background-color: rgb(248, 246, 246);
     padding: 5px;
}

.search{
    width: 250px;
    padding: 5px;
    font-size: 14px;
    font-weight: bold;
    background-color: rgb(192, 188, 188);
    border-radius: 25px;
    color: rgb(0, 0, 0);
    border: 0px;
}

.error{
    margin-top: 50vh;
    color: red;
}
</style>