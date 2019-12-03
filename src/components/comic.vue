<template>
    <div class="comic">
        <div class="contenedor-comic">
            <div class="contenido imagen">
                <img :src= "url" alt="">
            </div>
                <div class="contenido texto" v-for= "libro in novelagrafica" :key= "libro.id">
                    <h2>{{libro.title}}</h2>
                    <div class="texto--contenedor">
                        <p class="texto--contenedor__titulo">Description:</p>
                        <p class="texto--contenedor__liteartura"
                        v-if= "libro.description != null"
                            >{{libro.description}}</p>
                        <p class="texto--contenedor__liteartura" v-else
                            >El contedino de este archivo es clasificado y carece de la autoridad suficiente acceder a el. Para mas información onsulte el serviciod e atención al cliente de S.H.I.E.L.D ¡HEIL HIDRA!</p>
                   </div>
                    <div class="texto--contenedor">
                        <p class="texto--contenedor__titulo">Characters:</p>
                        <ul 
                            v-if= "libro.characters.available != 0">
                                <li class="texto--contenedor__liteartura" 
                                v-for= "personaje in libro.characters.items" 
                                :key="personaje.name">{{personaje.name}}</li>
                        </ul>
                        <p class="texto--contenedor__liteartura" v-else>
                        Lo sentimos, los personajes de esta historia se han convertido en polvo por culpa de Thanos. Le rogamos paciencia, los vengadores estan trabajando para solucionarlo. </p>
                    </div>
                    <div class="texto--contenedor">
                        <p class="texto--contenedor__titulo">Price:</p>
                        <div v-for= "precio in libro.prices" :key="precio.price">
                            <p class="texto--contenedor__liteartura"
                            v-if= "precio.price != 0 || null">
                            {{precio.price}}</p>
                            <p class="texto--contenedor__liteartura" v-else>
                            ¡GRATIS!</p>
                        </div>
                    </div>
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
 name: "comic",
    data() {
        return{
 /* En la variable libroid se recoge el parametro pasado desde la pagina de comics 
 a trabes del enrutador */ 
            libroid: this.$route.params.id,
            novelagrafica: [],
            url: '',
            size: 'portrait_uncanny.jpg',
            error: {
                status: false,
                message: ''
            },
        }
    },
    methods: {
        getComic: function(){ 
            axios.get(`https://gateway.marvel.com:443/v1/public/comics/${this.libroid}?ts=${timeset}&apikey=${publicKey}&hash=${hash}`)
            .then((result) =>{
                result.data.data.results.forEach(item => {
                    this.novelagrafica.push(item)
                    this.url = `${item.thumbnail.path}/${this.size}`
                })
            }).catch((error) => {
                this.error.status = true;
                this.error.message = 'Sorry. SHIELD is under attack right now. Try again later';
            })
        } 
    },
    created() {
        this.getComic()
    } 
}
</script>


<style>

.contenedor-comic{
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
}

h2{
    margin: 0px
}

.texto{
    min-height: 454px;
    flex-direction: column;
    justify-content: space-between;
    font-family: 'font', sans-serif;
    padding-right: 50px;
    color: rgb(233, 233, 193);
    
}

.texto--contenedor__titulo{
    size: 20px;
    font-weight: bold;

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

</style>