<template>
    <div class="character">
        <h2 class="error" v-if="error.status">{{error.message}}</h2>
        <div v-else class="contenedor-character">
            <div class="contenido imagen">
                <img :src= "image.url" alt="">
            </div>
            <div class="contenido text">
                <h2>{{character.name}}</h2>
                <p v-if= "character.description != ''">
                        {{character.description}}</p>
                <p v-else>
                        El contedino de este archivo es clasificado y carece de la autoridad suficiente acceder a el. Para mas información consulte el serviciod e atención al cliente de S.H.I.E.L.D ¡HEIL HIDRA!</p>
                <router-link class="enrutador" to= '/'>
                    <button class="btn-volver">Inicio</button>
                </router-link>  
            </div>
        </div>
    </div>
</template>

<script>
import {publicKey, hash, timeset} from '../keys';
import axios from 'axios'

export default {
 name: "character",
    data() {
        return{
            urlBase: this.$route.params.url,
            character: {},
            image: {
                url: '',
                size: 'portrait_incredible.jpg'
            },
            error: {
                status: false,
                message: ''
            },
        }
    },
    methods: {
        getCharacter: function(){ 
            axios.get(`${this.urlBase}?ts=${timeset}&apikey=${publicKey}&hash=${hash}`)
            .then((result) =>{
                result.data.data.results.forEach(item => {
                    this.character = item
                    this.image.url = `${item.thumbnail.path}/${this.image.size}`
                })
            }).catch(() => {
                this.error.status = true;
                this.error.message = 'Sorry. SHIELD is under attack right now. Try again later';
            })
        } 
    },
    created() {
        this.getCharacter()
    } 
}
</script>

<style>

.contenedor-character{
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 50px
}

.contenido{
    flex: 50%;
    text-align: justify;
    display: flex;
}

.imagen{
    justify-content: center;
    align-self: flex-start;
}

h2{
    margin: 0px
}

.text{
    min-height: 300px;
    flex-direction: column;
    justify-content: space-between;
    font-family: 'font', sans-serif;
    padding-right: 50px;
    color: rgb(233, 233, 193);
    
}

.enrutador{
    align-self: center;
}

.btn-volver{
    width: 200px;
    padding: 10px;
    font-size: 16px;
    font-weight: bold;
    background-color: transparent;
    border-radius: 25px;
    align-self: center;
    color: rgb(233, 233, 193);
}

.error{
    margin-top: 50vh;
    color: red;
}
</style>